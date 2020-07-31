---
title: "Integrating AppSignal into Plug"
---

-> **Note**: Support for custom namespaces was added in version 1.3.0 of the
   AppSignal for Elixir package.

The AppSignal for Elixir package integrates with Plug. To set up the
integration, please follow our [installation guide](/elixir/installation.html).

This page describes how to set up AppSignal in a Plug application, and how to
add instrumentation for events within requests. For more information about
custom instrumentation, read the [Elixir
instrumentation](/elixir/instrumentation/index.html) documentation.

More information can be found in the [AppSignal Hex package
documentation][hex-appsignal].

## Table of Contents

- [Getting started](#getting-started)
- [Incoming HTTP requests](#incoming-http-requests)
- [Custom instrumentation](#custom-instrumentation)
- [Instrumentation for included Plugs](#instrumentation-for-included-plugs)

## Getting started

Since version 2.0, the Plug integration is moved to a separate library named
`:appsignal_plug`, which depends on the main `:appsignal` library. To use
AppSignal in a Plug app, add `:appsignal_plug` to your dependencies. You
can then remove the `:appsignal` dependency.

``` elixir
defmodule AppsignalPlugExample.MixProject do
  # ...

  defp deps do
    [
      {:plug_cowboy, "~> 2.0"},
      {:appsignal_plug, "~> 2.0.0"}
    ]
  end

  # ...
end
```

## Incoming HTTP requests

We'll start out with a small Plug app that accepts `GET` requests to `/` and
returns a welcome message. To start logging HTTP requests in this app, we'll
use the `Appsignal.Plug` module.

``` elixir
defmodule AppsignalPlugExample do
  use Plug.Router
  use Appsignal.Plug # <- Add this

  plug :match
  plug :dispatch

  get "/" do
    send_resp(conn, 200, "Welcome")
  end
end
```

This will create a transaction for every HTTP request which is performed on the 
endpoint.

## Custom instrumentation

Although `Appsignal.Plug` will start transactions for you, it won't instrument
events in your app just yet. To do that, we need to add some custom
instrumentation.

This example app looks like the one we used before, but it has a slow function
(aptly named `slow/0`) we'd like to add instrumentation for. To do that, we need
to use the `Appsignal.instrument/2-3` helper in our called function:

``` elixir
defmodule AppsignalPlugExample do
  use Plug.Router

  plug :match
  plug :dispatch

  get "/" do
    slow()
    send_resp(conn, 200, "Welcome")
  end

  defp slow do
    Appsignal.Instrument("slow", fn ->
      :timer.sleep(1000)
    end)
  end

  use Appsignal.Plug
end
```

This will add an event for the `slow/0` function to the current transaction
whenever it's called. For more information about custom instrumentation, read
the [Elixir instrumentation](/elixir/instrumentation/index.html) documentation.

## Instrumentation for included Plugs

Exceptions in included Plugs are automatically caught by AppSignal, but
performance samples need to be set up manually using the custom instrumentation
helpers or decorators.

### Plug instrumentation with decorators

To add instrumentation to Plugs, use the `Appsignal.instrument/2` function:

``` elixir
defmodule SlowPlugWithDecorators do
  import Plug.Conn

  def init(opts), do: opts

  def call(conn, _) do
    Appsignal.instrument("SlowPlugWithDecorators", fn ->
      :timer.sleep(1000)
      conn
    end)
  end
end
```
[hex-appsignal]: https://hexdocs.pm/appsignal/
