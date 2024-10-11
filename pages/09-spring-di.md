# Spring DI

```kotlin
interface ServiceA

@Service
class ServiceAImpl(private val serviceB: ServiceB): ServiceA
```
<br>

```kotlin
interface ServiceB

@Service
class ServiceBImpl(private val serviceA: ServiceA): ServiceB // Naa :)?
```

<arrow v-click x1="140" y1="320" x2="315" y2="225" color="#953" width="2" arrowSize="1" />
<br>
<div v-click>

```kotlin

@ComponentScan('a.c.m.e') // Runtime scan 🤹!
class Application
```

</div>

