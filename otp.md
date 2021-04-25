---
title: OTP
category: Elixir
tags: [Featured, WIP]
updated: 2021-04-25
weight: -4
---

Intro
-------------------------------------

### spawn

```elixir
# pid = spawn(fun)
pid = spawn(fn -> IO.puts("Hello!") end)
pid = spawn_link(fn -> IO.puts("Hello!") end)
```

```elixir
# pid = spawn(module, function, params)
pid = spawn(SomeModule, :some_function, [1, 2, 3])
pid = spawn_link(SomeModule, :some_function, [1, 2, 3])
```


### send

```elixir
send(pid, message)
```


### receive

```elixir
receive do
  message -> IO.puts(message)
after
  5000 -> IO.puts(:stderr, "No message in 5 seconds!")
end
```

GenServer
-------------------------------------

```elixir
defmodule SomeServer do
  use GenServer

  @impl GenServer
  def init(initial_state) do
    {:ok, initial_state}
  end

  def start_link(_) do
    {:ok, pid} = GenServer.start_link(module, initial_state, options)
  end

  def some_call(pid, msg) do
    response = GenServer.call(pid, msg, timeout \\ 5000)
  end

  def some_cast do
    :ok = GenServer.cast(pid, msg, timeout \\ 5000)
  end

  def reply do
    :ok = GenServer.reply(from, msg)
  end
end
```

### start_link

```elixir
{:ok, pid} = GenServer.start_link(module, initial_state, options)
```

#### Options

| Name | Description |
| :------------- | :------------- |
| `:name` | used for name registration |
| `:timeout` | time the server is allowed to spend on initialization |


### handle_call callback

```elixir
@impl GenServer
def handle_call(request, from, state) do
  {:reply, response, new_state}
end
```

#### Responses

| Name | Description |
| :------------- | :------------- |
| `{:reply, reply, new_state}` | sends the response `reply` and continues the loop |
| `{:noreply, new_state}` | Does not send a response and continues the loop. The response must be sent with `reply/2` |


### handle_cast callback

```elixir
@impl GenServer
def handle_cast(request, state) do
  {:noreply, new_state}
end
```

### handle_info callback

```elixir
@impl GenServer
def handle_info(request, state) do
  {:noreply, new_state}
end
```

### handle_continue callback

```elixir
@impl GenServer
def handle_continue(continue, state) do
  {:noreply, new_state}
end
```

[Elixir School - handle_continue](https://elixirschool.com/blog/til-genserver-handle-continue/)

Exits
-------------------------------------

### Trapping exits

```elixir
Process.flag(:trap_exit, true)
{:EXIT, from_pid, exit_reason}
{reason, where}
```

### Monitors

```elixir
monitor_ref = Process.monitor(target_pid)
{:DONW, monitor_ref, :process, from_pid, exit_reason}
Process.demonitor(monitor_ref)
```


Supervisor
-------------------------------------

```elixir
{:ok, supervisor_pid} = Supervisor.start_link(children, options)
```

#### Options

| Name | Description |
| :------------- | :------------- |
| `:strategy` | the supervision strategy       |
| `:max_restarts` | maximum number of restarts allowed in a time frame. Default: 3 |
| `:max_seconds` | the time frame for `:max_restarts`. Default: 5 |
| `:name` | a name to register the supervisor process |

#### Strategies

| Name | Description |
| :------------- | :------------- |
| `:one_for_one` | only that process is restarted |
| `:one_for_all` | all other child processes are terminated and then all are restarted |
| `:rest_for_one` | the terminated child and the rest of the children started after ir are terminated and restarted |

### Child specification

```elixir
%{
    id: Module,
    start: {Module, :start_link, [nil]},
    type: :worker,
    restart: :permanent,
    shutdown: 5_000
}
```

#### Options

| Name | Description |
| :------------- | :------------- |
| `:id` | identifies the child, defaults to the given module |
| `start` | tuple of mod, fun args       |
| `:type` | Item Two       |
| `restart` | defines when a terminated child process should be restarted, defaults to `permanent`       |
| `shutdown` | defines how a child should be terminated       |

#### Shutdown values

| Name | Description |
| :------------- | :------------- |
| `:brutal_kill` | child process is immediately terminated |
| integer` | the amount of time the supervisor will wait |
| `:infinity` | waits indefinitely |

#### Restart values

| Name | Description |
| :------------- | :------------- |
| `:permanent` | child process is always restarted |
| `:temporary` | child process is never restarted |
| `:transient` | child process is restarted only if it terminates abnormally |


[Hexdocs - Child specification](https://hexdocs.pm/elixir/Supervisor.html#module-child-specification)

### With a module

```elixir
defmodule Module do
  use Supervisor

  def start_link do
    Supervisor.start_link(__MODULE__, nil)
  end

  @impl Supervisor
  def init(_) do
    Supervisor.init(children, options)
  end
end
```

Registry
-------------------------------------

```elixir
{:ok, pid} = Registry.start_link(name: :my_registry, keys: :unique)
{:ok, pid} = Registry.register(:my_registry, {:my_worker, 1}, nil)
[{pid, value}] = Registry.lookup(:my_registry, {:my_worker, 1})
```

### With a module

```elixir
defmodule ModuleRegistry do
  def start_link do
  end

  def via_tuple(key) do
    {:via, Registry, {__MODULE__, key}}
  end

  def child_spec(_) do
    Supervisor.child_spec(
      Registry,
      id: __MODULE__,
      start: {__MODULE__, start_link, []}
    )
  end
end
```

Task
-------------------------------------

```elixir
task = Task.async(fun)
Task.await(task)
```


Agent
-------------------------------------

```elixir
{:ok, pid} = Agent.start_link(fun)
Agent.get(pid, fun)
:ok = Agent.update(pid, fun)
:ok = Agent.cast(pid, fun)
```
