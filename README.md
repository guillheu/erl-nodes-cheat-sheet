# Erlang Node Operations Cheat Sheet

## Starting Nodes

### Basic Node Start
```bash
erl -name node1@hostname      # Full hostname
erl -sname node1             # Short hostname
```

### Headless Nodes
```bash
erl -name node1@hostname -noshell -noinput    # Completely headless
erl -name node1@hostname -noshell             # No shell but can receive input
```

### Node Options
```bash
-setcookie mycookie                           # Set node cookie
-kernel inet_dist_listen_min 9100             # Set min port
-kernel inet_dist_listen_max 9200             # Set max port
```

## Peering Nodes

### Manual Peering
```erlang
net_adm:ping('node1@hostname').              % Returns pong/pang
nodes().                                     % List connected nodes
erlang:disconnect_node('node1@hostname').     % Disconnect a node
```

### Remote Peering via RPC
```erlang
rpc:call('node1@hostname', net_adm, ping, ['node2@hostname']).
```

## RPC (Remote Procedure Calls)

### Basic RPC
```erlang
rpc:call(Node, Module, Function, ArgumentsList).
rpc:call('node1@hostname', crypto, strong_rand_bytes, [10]).
```

### Async RPC
```erlang
rpc:cast(Node, Module, Function, ArgumentsList).
```

### Multi-node RPC
```erlang
rpc:multicall(Nodes, Module, Function, ArgumentsList).
rpc:multicall([node1@hostname, node2@hostname], os, timestamp, []).
```

## Module Operations

### Local Module Operations
```erlang
c(my_module).                                % Compile module
l(my_module).                                % Load/reload module
code:add_path("/path/to/beam/files").        % Add to code path
code:load_file(my_module).                   % Load specific module
code:load_abs("/path/to/my_module").         % Load from absolute path
code:is_loaded(my_module).                   % Check if module loaded
```

### File Path Wildcard Operations
```erlang
Paths = filelib:wildcard("/path/*").                                   % Create multipath from glob expression
[code:add_path(Path) || Path <- Paths].                                % Add all paths to code path
[code:add_path(P) || P <- filelib:wildcard("/path/to/directory/*")].   % As a compact single expression
```

### Remote Module Operations
```erlang
% Copy module to remote node
rpc:call(Node, code, add_path, ["/path/to/beam"]).
rpc:call(Node, code, load_file, [my_module]).

% Execute remote function
rpc:call(Node, my_module, my_function, [Arg1, Arg2]).
```

## Slave Nodes

### Start Slave Node
```erlang
% From Erlang shell
slave:start(Host, NodeName).
slave:start_link(Host, NodeName).

% With options
slave:start(Host, NodeName, Args).
slave:start("localhost", node1, "-setcookie mycookie").
```

### Start Slave with Custom Parameters
```erlang
% Start with specific working directory and arguments
slave:start_link(Host, NodeName, Args, WorkingDir).
```

## Node Information

### Query Node Status
```erlang
nodes().                    % List connected nodes
erlang:nodes(connected).    % List connected nodes
erlang:nodes(hidden).       % List hidden nodes
node().                     % Current node name
is_alive().                % Check if node is alive
```

### Node Configuration
```erlang
erlang:get_cookie().       % Get current cookie
erlang:set_cookie(Node, Cookie).  % Set cookie for node
net_kernel:get_net_ticktime().    % Get net tick time
```

## Distribution Settings
```erlang
% In vm.args or command line
-kernel inet_dist_listen_min 9100
-kernel inet_dist_listen_max 9200
-kernel dist_auto_connect once
-kernel sync_nodes_timeout 10000
```

Would you like me to expand on any of these sections?
