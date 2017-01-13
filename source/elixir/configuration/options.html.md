---
title: "AppSignal Elixir configuration options"
---

The following list includes all configuration options with the name of the
environment variable and the name of the key in the configuration file.

## `APPSIGNAL_ACTIVE` / `:active`

- Value: Boolean `true`/`false`. Default: `false`

Whether AppSignal is active for this environment, can be `true` or `false`.

## `APPSIGNAL_APP_NAME` / `:name`

- Value: String

Name of your application as it should be displayed on AppSignal.com.

## `APPSIGNAL_DEBUG` / `:debug`

- Value: Boolean `true`/`false`. Default: `false`

Enable debug logging, this is usually only needed on request from support.

## `APPSIGNAL_ENABLE_HOST_METRICS` / `:enable_host_metrics`

- Value: Boolean `true`/`false`. Default: `true`

Set this to `false` to disable [host metrics](/metrics/host.html).

## `APPSIGNAL_APP_NAME` / `:env`

- Value: String. Default: `:dev`

The AppSignal environment in which the configuration is stored.

Other valid values are `:test` and `:prod`.

-> This environment variable name for this config was renamed from
`APPSIGNAL_ENVIRONMENT` to `APPSIGNAL_APP_ENV` after version `0.9.2`.

## `APPSIGNAL_FILTER_PARAMETERS` / `:filter_parameters`

- Value: Array<String>. Default: `[]`

List of parameter keys that should be ignored using AppSignal filtering. Their
values will be replaced with `FILTERED` when transmitted to AppSignal. You can
configure this with a list of keys in the configuration file.

## `APPSIGNAL_HTTP_PROXY` / `:http_proxy`

- Value: String. Default: `""`.

If you require the agent to connect to the Internet via a proxy set the
complete proxy URL in this configuration key.

## `APPSIGNAL_IGNORE_ACTIONS` / `:ignore_actions`

- Value: Array<String>. Default: `[]`.

List of actions that will be ignored, everything that happens including
exceptions will not be transmitted to AppSignal.

## `APPSIGNAL_IGNORE_ERRORS` / `:ignore_errors`

- Value: Array<String>. Default: `[]`.

List of error classes that will be ignored. Any error raised with this
error type will not be transmitted to AppSignal.

## `APPSIGNAL_LOG_PATH` / `:log_path`

- Value: String. Default: `/tmp/appsignal.log`

Override the location of the path where the AppSignal log file will be written
to.

## `APPSIGNAL_PUSH_API_ENDPOINT` / `:endpoint`

- Value: String. Default: `https://push.appsignal.com`

Configure the endpoint to send data to AppSignal.

## `APPSIGNAL_PUSH_API_KEY` / `:push_api_key`

- Value: String

The [Push API key](/appsignal/terminology.html#push-api-key) to authenticate
with when sending data to AppSignal.

## `APP_REVISION` / `:revision`

- Value: String

Version (revision) of your application to register.

## `APPSIGNAL_RUNNING_IN_CONTAINER` / `:running_in_container`

- Value: Boolean `true`/`false`. Default: `false`

AppSignal expects to be running on the same machine between different deploys.
Set this key to `true` if you use a container based deployment system such as
Docker.

## `APPSIGNAL_WORKING_DIR_PATH` / `:working_dir_path`

- Value: String. Default: detected by agent

Override the location where the AppSignal Ruby gem can store temporary files.
Use this is if the default location is not suitable.

If you are running multiple applications using AppSignal on the same server,
use this configuration option to select different working directories for every
AppSignal instance, otherwise the two instances could conflict with one
another.

```elixir
# config/config.exs
config :appsignal, :config,
  working_dir_path: "/tmp/project_1/"
```