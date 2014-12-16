# Singleton

```
    public function singleton($key, $value){
        $this->set($key, function ($c) use ($value) {
            static $object;

            if (null === $object) {
                $object = $value($c);
            }

            return $object;
        });
    }
    
    $this->container->singleton('request', function ($c) {
        return new \Slim\Http\Request($c['environment']); // Mark1
    });   
    
    $call = $this->request;
    $call($config_arr); // Mark1 called
```

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
