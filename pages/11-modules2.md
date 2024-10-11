# Modules

```plantuml
@startuml

[new-module] -u-> [legacy-module] : use
[new-module] -> [new-api-module] : use
[legacy-module] -> [new-api-module] : use

@enduml
```
<br>
<div v-click>

```plantuml
@startuml

[new-module] -u-> [legacy-module] : use
[new-module] -> [new-api-module] : use
[legacy-module] -> [new-api-module] : use
[customer-module] -d-> [new-api-module] : use

@enduml
```

</div>

::right::

# Dependencies
| **Module**      | **Dep-Type**  	 | **Module** 	    |
|-----------------|-----------------|-----------------|
| new-module 	    | compileTime     | legacy-module	  |
| new-module 	    | compileTime     | new-api-module	 |
| legacy-module 	 | compileTime     | new-api-module	 |
| legacy-module 	 | runTime         | new-module	     |

<br>

## Ports and Adapters
The new API module defines its own domain interfaces and entities, implemented by API adapters in the new module.