---
title: "Create a multi-tenant, whitelabel application in Elixir & Phoenix part II: Dynamic subdomain routing"
author: "makisotman"
layout: post
tag:
- elixir
- phoenix
- multi-tenant
- multi-tenancy
- whitelabel application
category: blog
image: assets/images/blog-image.png
---

___This blog post originally appeared on the [Red Squirrel Blog](https://medium.com/redsquirrel-tech/create-a-multi-tenant-whitelabel-application-in-elixir-phoenix-part-ii-dynamic-subdomain-a32513f6fac0).___

In the [previous post]({% post_url 2022-11-19-multi_tenant_whitelabel_application_part_1 %}) we outlined what a multi-tenant whitelabel application is. We also got our hands dirty by creating a new Phoenix application where we added the foundations for the app to be accessible via different sub-domains as well as a root domain.

The posts in these series are:

- [Part I: Working with subdomains]({% post_url 2022-11-19-multi_tenant_whitelabel_application_part_1 %})
- Part II: Dynamic subdomain routing
- Part III: Adding accounts tenants

In this article we will extend our app to use a custom router and dispatch subdomain requests to a separate controller which then loads different pages than our root domain.

### Adding subdomain routing

In the last post we went through the concept of adding a custom plug to capture a given subdomain and store it in the connection struct (`conn`). We then used that information to display *different data on the same page*. We'll take this a step further now by adding the necessary code to display a *different page altogether*.

Our code from last time uses the same router and serves the same page albeit with different data for requests coming from both the root domain and a subdomain. We want requests coming from a subdomain to be handled by a different router.

Let's head back to our project and add a new file under the `lib/fresco_web` directory called `subdomain_router.ex`. Add the following code inside of it:

```elixir
# lib/fresco_web/subodomain_router.ex

defmodule FrescoWeb.SubdomainRouter do
  use FrescoWeb, :router

  pipeline :browser do
    plug :accepts, ["html"]
    plug :fetch_session
    plug :fetch_live_flash
    plug :put_root_layout, {FrescoWeb.LayoutView, :root}
    plug :protect_from_forgery
    plug :put_secure_browser_headers
  end

  pipeline :api do
    plug :accepts, ["json"]
  end

  scope "/", FrescoWeb.Subdomain do
    pipe_through :browser

    get "/", PageController, :index
  end
end
```

You might think this looks identical to the existing router found under `lib/fresco_web/router.ex` and you are largely correct. The main difference is on the line starting with `scope`. This is the code from `lib/fresco_web/router.ex`:

```elixir
scope "/", FrescoWeb do
  pipe_through :browser

  get "/", PageController, :index
end
```

In our new router, we're scoping the incoming queries to a different context called `Subdomain` :

```elixir
scope "/", FrescoWeb.Subdomain
```

**Note:** You might be thinking *"Why do I need another router? Can't I do all of that in same router?"*. You probably can but there are some good reasons to have separate routers:

- Clearer code organisation and separation
- Smaller modules
- Easier/quicker to conceptually parse vs a single file with mixed responsibilities
- Custom configuration (i.e pipelines, plugs etc) can be added that's only applicable to routing subdomains or vice verca

Now that we have a subdomain router, let's use it. Open the endpoint file `lib/fresco_web/endpoint.ex` and change the line where we added our subdomain plug and change this code:

```elixir
plug FrescoWeb.Plugs.Subdomain
```

to this:

```elixir
plug FrescoWeb.Plugs.Subdomain, %{ subdomain_router: FrescoWeb.SubdomainRouter }
```

Previously our `Subdomain` plug was initialised without any additional options. Now we're passing additional options, in this case a map, containing our newly created `SubdomainRouter`. This means we also need to change our code in the plug subdomain file `lib/fresco_web/plugs/subdomain.ex` to make use of this change:

```elixir
defmodule FrescoWeb.Plugs.Subdomain do
  @behaviour Plug

  import Plug.Conn, only: [put_private: 3]

  # we will use the options parameter and merge the incoming options with the existing one
  def init(opts) do
    Map.merge(
      opts,
      %{ root_host: FrescoWeb.Endpoint.config(:url)[:host] }
    )
  end

# unpack subdomain_router argument
  def call(%Plug.Conn{host: host} = conn, %{root_host: root_host, subdomain_router: router}) do
    case extract_subdomain(host, root_host) do
      subdomain when byte_size(subdomain) > 0 ->
        put_private(conn, :subdomain, subdomain)
        |> router.call(router.init({})) # <--- call the router with the incoming connection
      _ ->
        conn
    end
  end

  defp extract_subdomain(host, root_host) do
    String.replace(host, ~r/.?#{root_host}/, "")
  end
end
```

Restart the Phoenix server, visit a subdomain like http://green.fresco.com:4000/ and you should be greeted with....an error:

![phoenix routing error](/assets/phoenix_whitelabel_series//phoenix_controller_error.png)

Remember this line `scope "/", FrescoWeb.Subdomain` in our `SubdomainRouter`? Since the requests are now scoped to a `Subdomain` namespace, our app is looking for a `PageController` in that namespace.

Create a new directory under `lib/fresco_web/` called `subdomain`:

```bash
mkdir lib/fresco_web/subdomain
```

Inside of that new directory, create a file called `page_controller.ex` and add the following code:

```elixir
defmodule FrescoWeb.Subdomain.PageController do
  use FrescoWeb, :controller

  def index(conn, _params) do
    render(conn, "index.html", %{subdomain: conn.private[:subdomain]})
  end
end
```

Refresh the page in the browser. Indeed, another error greets us:

![phoenix view error](/assets/phoenix_whitelabel_series//phoenix_view_missing.png)

The error, like before, it's telling us the next action we need to take. Since now our `Subdomain.PageController` is used, during the `render` function call it's trying to find a `View` (`PageView`) that's under the same namespace (`Subdomain`). Create a new directory called `subdomain` under `lib/fresco_web/views/` and a new a file inside of that called `page_view.ex`:

```bash
mkdir lib/fresco_web/views/subdomain
touch lib/fresco_web/views/subdomain/page_view.ex
```

and add the following code inside of it:

```elixir
defmodule FrescoWeb.Subdomain.PageView do
  use FrescoWeb, :view
end
```

Refresh the page http://green.fresco.com:4000/ and:

![phoenix view error](/assets/phoenix_whitelabel_series//phoenix_template_missing.png)

I'm not trolling you, I promise. The reason I'm taking you on this journey is to show the "invisible" connecting dots between the different components that would otherwise be less obvious had I given you all of the code up front and skipped the step by step process.

We're almost there, one more file:

```bash
# create recursively two directories: subdomain and inside of it, page
mkdir -p lib/fresco_web/templates/subdomain/page/

# the file our error is looking for
touch lib/fresco_web/templates/subdomain/page/index.html.heex
```

Inside our new file `lib/fresco_web/templates/subdomain/page/index.html.heex` add the following:

```html
<section class="phx-hero">
  <h1><%= gettext "Welcome to %{name}!", name: @subdomain %></h1>
  <p>Peace of mind from prototype to production</p>
</section>

<section class="row">
  <article class="column">
    <h2>Resources</h2>
    <ul>
      <li>
        <a href="https://hexdocs.pm/phoenix/overview.html">Guides &amp; Docs</a>
      </li>
      <li>
        <a href="https://github.com/phoenixframework/phoenix">Source</a>
      </li>
      <li>
        <a href="https://github.com/phoenixframework/phoenix/blob/v1.6/CHANGELOG.md">v1.6 Changelog</a>
      </li>
    </ul>
  </article>
</section>
```

Refresh the page at http://green.fresco.com:4000/ and voila:

![phoenix view error](/assets/phoenix_whitelabel_series//phoenix_subdomain_controller_success.png)

Let's refresh the other pages as well, our root domain http://fresco.com:4000/ and our other subdomain http://farm.fresco.com:4000/. You should see the sweet image of success:

![phoenix view error](/assets/phoenix_whitelabel_series//phoenix_subdomain_routing_success.png)

As you can see, our root domain apart from a different greeting it also displays the additional "Help" section since it's been handled by a different controller that renders a different template.

Let's address something you may have noticed in your server logs back in the terminal. More specifically this error:

```bash
[info] GET /
[debug] Processing with FrescoWeb.Subdomain.PageController.index/2
  Parameters: %{}
  Pipelines: [:browser]
[info] Sent 200 in 1ms
[debug] Processing with FrescoWeb.PageController.index/2
  Parameters: %{}
  Pipelines: [:browser]
[error] #PID<0.678.0> running FrescoWeb.Endpoint (connection #PID<0.647.0>, stream id 8) terminated
Server: farm.fresco.com:4000 (http)
Request: GET /
** (exit) an exception was raised:
    ** (Plug.Conn.AlreadySentError) the response was already sent
        (phoenix 1.6.15) lib/phoenix/controller.ex:570: Phoenix.Controller.put_root_layout/2
        (fresco 0.1.0) FrescoWeb.Router.browser/2
        (fresco 0.1.0) lib/fresco_web/router.ex:1: FrescoWeb.Router.__pipe_through0__/1
        (phoenix 1.6.15) lib/phoenix/router.ex:346: Phoenix.Router.__call__/2
        (fresco 0.1.0) lib/fresco_web/endpoint.ex:1: FrescoWeb.Endpoint.plug_builder_call/2
        (fresco 0.1.0) lib/plug/debugger.ex:136: FrescoWeb.Endpoint."call (overridable 3)"/2
        (fresco 0.1.0) lib/fresco_web/endpoint.ex:1: FrescoWeb.Endpoint.call/2
        (phoenix 1.6.15) lib/phoenix/endpoint/cowboy2_handler.ex:54: Phoenix.Endpoint.Cowboy2Handler.init/4
        (cowboy 2.9.0)
```

Remember the change we did in our endpoint file where we added our `Subdomain` plug right before the `Router` plug:

```elixir
plug FrescoWeb.Plugs.Subdomain, %{ subdomain_router: FrescoWeb.SubdomainRouter }
plug FrescoWeb.Router
```

Because our `Subdomain` plug now calls our new `SubdomainRouter` a response
is sent already from the server. Our Subdomain plug then returns the `conn` struct which is then passed to the `FrescoWeb.Router` which in turn tries to send a response as well causing the above error. We can fix this error by using the [`halt`](https://hexdocs.pm/plug/Plug.Conn.html#halt/1) function to prevent any further plus futher down the pipeline (i.e `FrescoWeb.Router`) from executing:

```elixir
defmodule FrescoWeb.Plugs.Subdomain do
  @behaviour Plug

  import Plug.Conn, only: [put_private: 3, halt: 1] # import the function

...

  def call(%Plug.Conn{host: host} = conn, %{root_host: root_host, subdomain_router: router}) do
    case extract_subdomain(host, root_host) do
      subdomain when byte_size(subdomain) > 0 ->
        put_private(conn, :subdomain, subdomain)
        |> router.call(router.init({}))
        |> halt() # <--- halt further execution
      _ ->
        conn
    end
  end

...

end
```

That's it, we did it. We removed the final error and our app is capable of handling requests from different domains and passing them to the appropriate router to handle. There is a lot of information to take in and explore further in this post so give yourself a pad on the back and a well-deserved break.

In our next post we'll take this ability to dynamically display custom content a step further with the introduction of user accounts aka tenants!
