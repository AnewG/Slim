# Flash

```
init -> $msg['prev'] = $_SESSION;
now  -> $msg['now'] // will display info on current request
keep -> $msg['next'] = $msg['prev'];
save -> $_SESSION = $msg['next']; // session info pass to next request
```

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

# Client-Cache

```

    public function lastModified($time){
        if (is_integer($time)) {
            $this->response->headers->set('Last-Modified', gmdate('D, d M Y H:i:s T', $time));
            if ($time === strtotime($this->request->headers->get('IF_MODIFIED_SINCE'))) {
                $this->halt(304);
            }
        } else {
            throw new \InvalidArgumentException('Slim::lastModified only accepts an integer UNIX timestamp value.');
        }
    }

    public function etag($value, $type = 'strong'){
        //Ensure type is correct
        if (!in_array($type, array('strong', 'weak'))) {
            throw new \InvalidArgumentException('Invalid Slim::etag type. Expected "strong" or "weak".');
        }

        //Set etag value
        $value = '"' . $value . '"';
        if ($type === 'weak') {
            $value = 'W/'.$value;
        }
        $this->response['ETag'] = $value;

        //Check conditional GET
        if ($etagsHeader = $this->request->headers->get('IF_NONE_MATCH')) {
            $etags = preg_split('@\s*,\s*@', $etagsHeader);
            if (in_array($value, $etags) || in_array('*', $etags)) {
                $this->halt(304);
            }
        }
    }
```
