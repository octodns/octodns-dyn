## [DEPRECATED] DynProvider provider for octoDNS

An [octoDNS](https://github.com/octodns/octodns/) provider that targets [Dyn](https://www.oracle.com/corporate/acquisitions/dyn/technologies/migrate-your-services/).

### Installation

#### Command line

```
pip install octodns-dyn
```

#### requirements.txt/setup.py

Pinning specific versions or SHAs is recommended to avoid unplanned upgrades.

##### Versions

```
# Start with the latest versions and don't just copy what's here
octodns==0.9.14
octodns-dyn==0.0.1
```

##### SHAs

```
# Start with the latest/specific versions and don't just copy what's here
-e git+https://git@github.com/octodns/octodns.git@9da19749e28f68407a1c246dfdf65663cdc1c422#egg=octodns
-e git+https://git@github.com/octodns/octodns-dyn.git@ec9661f8b335241ae4746eea467a8509205e6a30#egg=octodns_dyn
```

### Configuration

```yaml
providers:
  dyn:
    class: octodns_dyn.DynProvider
    # Your dynect customer name (required)
    customer: env/DYN_CUSTOMER
    # Your dynect username (required)
    username: env/DYN_USERNAME
    # Your dynect password (required)
    password: env/DYN_PASSWORD
    # Whether or not to support TrafficDirectors and enable GeoDNS
    # (optional, default is false)
    #traffic_directors_enabled: true
```

Note: due to the way dyn.tm.session.DynectSession is managing things we can only really have a single DynProvider configured. When you create a DynectSession it's stored in a thread-local singleton. You don't invoke methods on this session or a client that holds on to it. The client libraries grab their per-thread session by accessing the singleton through DynectSession.get_session(). That fundamentally doesn't support having more than one account active at a time. See DynProvider._check_dyn_sess for some related bits.

### Support Information

#### Records

All octoDNS record types are supported.

#### Dynamic

DynProvider does not support dynamic records.

### Development

See the [/script/](/script/) directory for some tools to help with the development process. They generally follow the [Script to rule them all](https://github.com/github/scripts-to-rule-them-all) pattern. Most useful is `./script/bootstrap` which will create a venv and install both the runtime and development related requirements. It will also hook up a pre-commit hook that covers most of what's run by CI.
