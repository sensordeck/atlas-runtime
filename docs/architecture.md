# Architecture

Atlas DSIL SDK V0.1 implements a minimal host-side closed loop:

Core Week 1 path:

`source -> parser -> daemon(runtime state)`

Shipped demo path present in the repository:

`source -> daemon -> snapshot -> ROS2 -> acceptance`

The active runtime is `host/dsild/daemon.py`.

The active Week 1 implementation is:

- source selection (`mock` by default, optional `serial` / `udp` / `file`)
- parser validation in `host/dsild/parser.py`
- runtime state consumption in `host/dsild/daemon.py`
- snapshot generation in `host/dsild/state_store.py`

The repository also ships a demo/acceptance path:

- ROS2 publishing in `host/ros2/bridge_node.py`
- acceptance in `scripts/acceptance_check.py`

Compatibility modules such as `host/dsild/runtime.py`, `host/dsild/app.py`, `host/dsild/sync_engine.py`, and `host/dsild/ph_manager.py` remain in the repository, but they are not the active V0.1 execution path.
