# Maim_Message Router
This is the message router for Maim_Message, responsible for forwarding messages from one platform to another.

## class TargetConfig
This is the basic unit of routing configuration.

```python
@dataclass
class TargetConfig:
    url: str
    token: Optional[str] = None

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "TargetConfig":
```

`url`: Routing target address.

`token`: Authentication key.

## class RouteConfig
This is the routing configuration class, responsible for indicating the direction of routing.

```python
@dataclass
class RouteConfig:
    route_config: Dict[str, TargetConfig] = None

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "RouteConfig":
```

`route_config`: A dictionary composed of multiple routing units.

## class Router
Router class, responsible for managing all routing configurations and connections **(only showing parts necessary for plugin development here)**.

```python
class Router:
    def __init__(self, config: RouteConfig):

    async def connect(self, platform: str):
        """Connect to specified platform"""

    async def run(self):
        """Run all client connections"""

    async def stop(self):
        """Stop all clients"""

    def register_class_handler(self, handler):

    async def send_message(self, message: MessageBase)
```

`__init__`: Initialize router instance, passing in routing configuration.

`connect`: Connect to specified platform. Automatically called when `run()` method is executed to connect all configured platforms.

`run`: Run all client connections, automatically connecting all configured platforms.

`stop`: Stop all client connections, automatically disconnecting all configured platforms.