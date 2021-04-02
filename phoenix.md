---
title: Phoenix framework
category: Elixir
tags: [Featured, WIP]
updated: 2020-12-29
weight: -5
---

Intro
-------------------------------------

### Creating a project

```shell
# mix phx.new <name>
mix phx.new my_app
```

#### Options

| `--live` | include LiveView |
| `--database` | possible values are: `postgres`, `mysql`, `mssql` |
| `--no-webpack` | |
| `--no-ecto` | |
| `--no-html` | |
| `--no-gettext` | |
| `--no-dashboard` | do not include Phoenix.LiveDashboard |
| `--install` | will fetch and install dependencies (including npm js) |
| `--no-install` | |

Generators
-------------------------------------

### migration

```shell
# mix ecto.gen.migration <migration name>
mix ecto.gen.migration add_posts_table
```

#### Other generators

- `cert`
- `channel`
- `embedded`
- `presence`
- `secret`

### schema

```shell
# mix phx.gen.schema <schema module> <plural model name> <attributes>
mix phx.gen.schema Blog.Post blog_posts title:string views:integer
```

This command will create the following files:

- a *schema file* in `lib/my_app/blog/post.ex`, with a `blog_posts` table
- a *migration file* (use `--no-migration` to skip this)

#### Attribute types

- `:string` - this is the default type if no type is given
- `:integer`
- `:float`
- `:decimal`
- `:boolean`
- `:map`
- `:array`
- `:references`
- `:text`
- `:date`
- `:time`
- `:time_usec`
- `:naive_datetime`
- `:naive_datetime_usec`
- `:utc_datetime`
- `:utc_datetime_usec`
- `:uuid`
- `:binary`
- `:datetime` - An alias for `:naive_datetime`

`user_id:references:users` will result in a migration with an `:integer` column of `:user_id` and create an index.

`tags:array:string` will create an array type if the database supports it.

Unique columns can be generated with `title:unique` or `unique_int:integer:unique`

#### Options

| `--table cms_posts` | change the name of the generated table |
| `--no-migration` | |


### context

```shell
# mix phx.gen.context <context> <schema> <plural schema> <attributes>
mix phx.gen.context Accounts User users name:string age:integer
```

This command will create the following files:

- a context module in `accounts.ex`
- a schema file in `accounts/user.ex`, with a `users` table
- a migration file

#### Options

| `--table cms_posts` | change the name of the table generated |
| `--no-schema` | |
| `--no-migration` | |


### html

```shell
# mix phx.gen.html <context> <schema> <plural schema> <attributes>
mix phx.gen.html Accounts User users name:string age:integer
```

This command will create the following files:

- a context module in `lib/app/accounts.ex` for the accounts API
- a schema file in `lib/app/accounts/user.ex`, with an users table
- a view file in `lib/app_web/views/user_view.ex`
- a controller file in `lib/app_web/controllers/user_controller.ex`
- default CRUD templates in `lib/app_web/templates/user`

#### Options

| `--no-context` | |
| `--no-schema` | |


### json

```shell
# mix phx.gen.html <context> <schema> <plural schema> <attributes>
mix phx.gen.json Accounts User users name:string age:integer
```

This command will create the following files:

- a context module in `lib/app/accounts.ex` for the accounts API
- a schema file in `lib/app/accounts/user.ex`, with an users table
- a view file in `lib/app_web/views/user_view.ex`
- a controller file in `lib/app_web/controllers/user_controller.ex`

#### Options

- `--no-context`
- `--no-schema`


### live

```shell
# mix phx.gen.live <context> <schema> <plural schema> <attributes>
mix phx.gen.live Accounts User users name:string age:integer
```

This command will create the following files:

- a context module in `lib/app/accounts.ex` for the accounts API
- a schema file in `lib/app/accounts/user.ex`, with an users table
- a view file in `lib/app_web/views/user_view.ex`
- a LiveView in `lib/app_web/live/user_live/show_live.ex`
- a LiveView in `lib/app_web/live/user_live/index_live.ex`
- a LiveComponent in `lib/app_web/live/user_live/form_component.ex`
- a LiveComponent in `lib/app_web/live/modal_component.ex`
- a helpers modules in l`ib/app_web/live/live_helpers.ex`

#### Options

| `--no-context` | |
| `--no-schema` | |

Dynamic Forms
-------------------------------------

- [Dynamic Forms with Phoenix](https://blog.plataformatec.com.br/2016/09/dynamic-forms-with-phoenix/)

Resources
-------------------------------------

### Blog posts

- [Bootstrap native with Phoneix](https://dashbit.co/blog/using-bootstrap-native-with-live-view)
- [Optimizing User Experience with LiveView](https://dockyard.com/blog/2020/12/21/optimizing-user-experience-with-liveview)
