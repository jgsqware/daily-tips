### CPU profiling:
  ```go
  // Enable CPU Profiling if specified
  if *flagCPUProfilePath != "" {
    startCPUProfiling(*flagCPUProfilePath)
    defer stopCPUProfiling()
  }

    //Non returning method
  }

  func startCPUProfiling(path string) {
    f, err := os.Create(path)
    if err != nil {
      log.Fatalf("failed to create profile file: %s", err)
    }
    defer f.Close()

    pprof.StartCPUProfile(f)
    log.Info("started CPU profiling")
  }

  func stopCPUProfiling() {
    pprof.StopCPUProfile()
    log.Info("stopped CPU profiling")
  }
  ```

### WaitForSignals:

```go
  waitForSignals(os.Interrupt)
  log.Info("Received interruption, gracefully stopping ...")
  st.Stop()
  }

  func waitForSignals(signals ...os.Signal) {
  interrupts := make(chan os.Signal, 1)
  signal.Notify(interrupts, signals...)
  <-interrupts
  }
```

### Error definitions
```go
  var (
  	log = capnslog.NewPackageLogger("github.com/coreos/clair", "database")

  	// ErrTransaction is an error that occurs when a database transaction fails.
  	ErrTransaction = errors.New("database: transaction failed (concurrent modification?)")
  	// ErrBackendException is an error that occurs when the database backend does
  	// not work properly (ie. unreachable).
  	ErrBackendException = errors.New("database: could not query backend")
  	// ErrInconsistent is an error that occurs when a database consistency check
  	// fails (ie. when an entity which is supposed to be unique is detected twice)
  	ErrInconsistent = errors.New("database: inconsistent database")
  	// ErrCantOpen is an error that occurs when the database could not be opened
  	ErrCantOpen = errors.New("database: could not open database")

  	store *cayley.Handle
  )
```

### Mutex and Lock
```go
var (
	healthcheckersLock sync.Mutex
	healthcheckers     = make(map[string]Healthchecker)
)

// RegisterHealthchecker registers a Healthchecker function which will be part of Healthchecks
func RegisterHealthchecker(name string, f Healthchecker) {

	healthcheckersLock.Lock()
	defer healthcheckersLock.Unlock()

	if _, alreadyExists := healthcheckers[name]; alreadyExists {
		panic(fmt.Sprintf("Healthchecker '%s' is already registered", name))
	}
	healthcheckers[name] = f
}```
