# Leptus [![Build Status](https://travis-ci.org/s1n4/leptus.png?branch=master)](https://travis-ci.org/s1n4/leptus)

Leptus is an Erlang REST framework that runs on top of cowboy.

Leptus aims at simply creating RESTful APIs.

## Requirements

  * Erlang/OTP R15B or newer
  * [cowboy](https://github.com/extend/cowboy)
  * [jiffy](https://github.com/davisp/jiffy)

## Installation

Clone it and just run `make`

OR

If you want to use it as a dependency in your project add the following to your rebar configuration

```
{deps, [
        ...
        {leptus, ".*", {git, "git://github.com/s1n4/leptus.git", {branch, "master"}}}
       ]}.
```

## Quickstart

```erlang
-module(rq_handler).
-compile({parse_transform, leptus_pt}).

%% leptus callbacks
-export([init/3]).
-export([get/3]).
-export([terminate/3]).

init(_Route, _Req, State) ->
    {ok, State}.

get("/", _Req, State) ->
    {<<"Hello, leptus!">>, State};
get("/hi/:name", Req, State) ->
    Status = 200,
    Name = leptus_req:param(name, Req),
    Body = [{<<"say">>, <<"Hi">>}, {<<"to">>, Name}],
    {Status, json, Body, State}.

terminate(_Reason, _Req, _State) ->
    ok.
```

```
$ erl -pa ebin deps/*/ebin
```

```erlang
1> c(rq_handler).
2> Handlers = [{rq_handler, []}].
3> leptus:start_http(Handlers).
```

```
$ curl localhost:8000/hi/Leptus
{"say":"Hi","to":"Leptus"}
```

## License

MIT, see LICENSE file for more details.

## TODOs

* Add code upgrade functionality
* Add http streaming support
* Add hooks capability
* Add examples
* ...
