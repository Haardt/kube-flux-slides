# Packages

* Domain-A (Package)
  * Service | UseCase (Interface -> Port)
  * ServiceImpl | AppImpl (Class)
  * Repository (Interface -> Port)
  * adapter (Package)
    * ServiceRepository (Class -> Adapter, Driven)
    * RestController (Adapter, Driving)
* ...
* Domain-X (Package)
    * ...
    * adapter (Package)
      * ServiceRepository (Class -> Adapter, Driven)
      * RestController (Adapter, Driving)