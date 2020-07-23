---
title: "Ignore errors"
---

Sometimes an error is raised which AppSignal shouldn't send alert about. It's
not desired to capture an exception with a `try..rescue` block just to
prevent AppSignal from alerting you. Instead, the exception should be handled
by the framework the application is using.

To prevent AppSignal from picking up these errors and alerting you, you can add
exceptions that you want to ignore to the list of ignored errors in your
configuration.

More information about the [`ignore_errors`](/elixir/configuration/options.html#option-ignore_errors) configuration option.

```elixir
# config/appsignal.exs
use Mix.Config

config :appsignal, :config,
  otp_app: :appsignal_phoenix_example,
  name: "AppsignalPhoenixExample",
  push_api_key: "your-push-api-key",
  env: Mix.env,
  active: true,
  ignore_errors: ["VerySpecificError", "AnotherError"]
```

You can also configure ignore exceptions via an environment variable.

```bash
export APPSIGNAL_IGNORE_ERRORS="VerySpecificError,AnotherError"
```

Any exceptions defined here will not be sent to AppSignal and will not trigger
a notification.

Read more about [Exception
handling](/elixir/instrumentation/exception-handling.html).
