# Router

```
Slim group() called push group prefix
  |
  |__  if is recursive,push again
        |
        |__ Slim get() or post() called -> call mapRoute() , generate group prefix to $prefix(use foreach .=)
            put Slim/Route($prefix.$pattern,$callable) to Routers(AllSet)
            Also, Set Middleware(if pass)
            
        |
        |__ pop recursive group prefix
  |
  |__ pop top level group prefix(clear prefix for next call)
```
