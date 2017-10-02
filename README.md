# Conduit

Discover why functional languages, such as Elixir, are ideally suited to building applications following the command query responsibility segregation and event sourcing (CQRS/ES) pattern.

Conduit is a blogging platform, an exemplary Medium.com clone, built as a Phoenix web application.

This is the full source code to accompany the "[Building Conduit](https://leanpub.com/buildingconduit)" eBook.

This book is for anyone who has an interest in CQRS/ES and Elixir. It demonstrates step-by-step how to build an Elixir application implementing the CQRS/ES pattern using the [Commanded](https://github.com/slashdotdash/commanded) open source library.

---

MIT License

[![Build Status](https://travis-ci.org/slashdotdash/conduit.svg?branch=master)](https://travis-ci.org/slashdotdash/conduit)

---

## Getting started

Conduit is an Elixir 1.5 application using Phoenix 1.3 and PostgreSQL for persistence.

### Prerequisites

You must install the following dependencies before starting:

- [Git](https://git-scm.com/).
- [Elixir](https://elixir-lang.org/install.html) (v1.5 or later).
- [PostgreSQL](https://www.postgresql.org/) database (v9.5 or later).

### Configuring Conduit

1. Clone the Git repo from GitHub:

    ```console
    $ git clone https://github.com/slashdotdash/conduit.git
    ```

2. Install mix dependencies:

    ```console
    $ cd conduit
    $ mix deps.get
    ```

3. Create the event store database:

    ```console
    $ mix event_store.create
    ```

4. Create the read model store database:

    ```console
    $ mix do ecto.create, ecto.migrate
    ```

5. Run the Phoenix server:

    ```console
    $ mix phx.server
    ```

  This will start the web server on localhost, port 4000: [http://0.0.0.0:4000](http://0.0.0.0:4000)

This application *only* includes the API back-end, serving JSON requests.

You need to choose a front-end from those listed in the [RealWorld repo](https://github.com/gothinkster/realworld). Follow the installation instructions for the front-end you select. The most popular implementations are listed below.

- [React / Redux](https://github.com/gothinkster/react-redux-realworld-example-app)
- [Elm](https://github.com/rtfeldman/elm-spa-example)
- [Angular 4+](https://github.com/gothinkster/angular-realworld-example-app)
- [Angular 1.5+](https://github.com/gothinkster/angularjs-realworld-example-app)
- [React / MobX](https://github.com/gothinkster/react-mobx-realworld-example-app)

Any of these front-ends should integrate with the Conduit back-end due to their common API.

### Running on a cluster of nodes

To run Conduit on a cluster you need to start three terminals and run the following commands on each:

```console
$ PORT=4000 iex --name node1@127.0.0.1 -S mix phx.server
```

```console
$ PORT=4001 iex --name node2@127.0.0.1 -S mix phx.server
```

```console
$ PORT=4002 iex --name node3@127.0.0.1 -S mix phx.server
```

Or you can use the Erlang `sys.config` files to start each node. This approach will block node start up until all nodes have connected to each other (or the 30 second timeout has elapsed):

```console
$ PORT=4000 iex --name node1@127.0.0.1 --erl "-config cluster/node1.sys.config" -S mix phx.server
```

```console
$ PORT=4001 iex --name node2@127.0.0.1 --erl "-config cluster/node2.sys.config" -S mix phx.server
```

```console
$ PORT=4002 iex --name node3@127.0.0.1 --erl "-config cluster/node3.sys.config" -S mix phx.server
```

This will run Phoenix on each of the local nodes, running on ports 4000, 4001, and 4002 respectively. You can connect a front-end to *any* of the backends. The Conduit aggregates, event handlers, and process managers will be distributed amongst all available nodes.

The config specifies that at least two nodes are required at a minimum to form a quorum. So you can stop/start any one node and the remaining two nodes will continue to serve requests. Fewer than two connected nodes will stop the application as it cannot form a quorum.

## Need help?

Please [submit an issue](https://github.com/slashdotdash/conduit/issues) if you encounter a problem, or need support.
