# Benchmarks of KrylovKit for Quantum Propagation


## Observations

### Chebychev

* With pre-allocation (pre-initialized propagator), propagating with 8 threads is only 2.3 times (?) faster than propagating with a single thread, 3.8 times with thread pinning.
* Same ratio with `propagate_trajectories` that initializes the propagator internally
* Disabling garbage collection has no significant effect

### KrylovKit

* Runtime of KrylovKit seems very competitive with Chebychev. They're not doing _exactly_ the same thing, though, so more testing will be necessary
* Just _enabling_ multiple threads causes a 30% slowdown
* `propagate_trajs` is surprisingly slow compared to `propagate_traj`, in single-threaded mode. For 8 times the workload, we would expect 8 × 30ms = 240ms, but we're actually getting 415 ms
* Propagating with 8 threads provides a speedup factor of 4.7 (5.1 with thread pinning)
* In single-task code, there seems to be a performance penalty to disabling garbage collection
* By deactivating garbage collection, that becomes a factor of 5.6 (5.5 with thread pinning)
