# npm-vwconnectidapi
NPM package for a We Connect ID API based on https://github.com/TA2k/ioBroker.vw-connect  
and https://github.com/nightsha-de/npm-vwconnectapi

Clone this repository to $path and use
```
sudo npm install -g npm-vwconnectidapi
```
to install.

### Example code:

```javascript
const api = require('npm-vwconnectidapi');
const idStatusEmitter = api.idStatusEmitter;
var log = new api.Log();
var vwConn = new api.VwWeConnect();
vwConn.setLogLevel("INFO"); // optional, ERROR (default), INFO, WARN or DEBUG
vwConn.setCredentials("YourEmail", "YourPassword");

idStatusEmitter.on('currentSOC', (soc) =>  {
        console.log(soc);
});

vwConn.getData()
  .then(() => {
    
    vwConn.setActiveVin("the VIN of your ID"); // must exist in vwConn.vehicles
    //vwConn.startClimatisation().then(...)
    //vwConn.stopClimatisation().then(...)
    //vwConn.stopCharging().then(...)
    vwConn.startCharging()
      .then(() => {
        log.info("Charging started");
      })
      .catch(() => {
        log.error("Error while starting the charging");
      })
      .finally(() => {
        log.info("Exiting ...");
        vwConn.onUnload();
        process.exit(1);
      });
  })
  .catch(() => {
    log.error("something went wrong");
    process.exit(1);
  });
```

### Methods supplied by the API:
All methods work with promises.

#### vwConn.setCredentials(user, password)
Login credentials. Pin is not needed for the ID cars.

#### vwConn.setLogLevel(logLevel)
Set/change the log level to "DEBUG", "INFO" or "ERROR" (default).

#### vwConn.getData()
Fills all data objects. If the process is not explicitly exited after getData (see example) it will regularly update the data in a given interval.

#### vwConn.setActiveVin(VIN)
Sets the VIN that is used for climatisation and charging. Setting is mandatory before clima or charging actions, VIN needs to exists in the vwConn.vehicles data.

#### vwConn.startClimatisation()
Start the air-conditioning.

#### vwConn.setClimatisation(temperature)
Set climatisation temperature.

#### vwConn.stopClimatisation()
Stop the air-conditioning.

#### vwConn.setChargingSettings(targetSOC, maxChargeCurrent)
Change the target SOC and the maximum charge current ("maximum" or "reduced") for charging.

#### vwConn.startCharging()
Start charging.

#### vwConn.stopCharging()
Stop charging.

#### Status changes emitted by the API:
'statusNotSafe' - car is parked and doors remain unlocked or windows opened for >5 minutes.  
'parked' - Car is parked. Emits parking position as argument.  
'notParked' - Car is on the move.  
'chargePurposeReached' - Target state of charge reached.  
'chargingStarted' - Charging started.  
'chargingStopped' - Charging stopped.  
'currentSOC' - Actuel state of charge changed. Emits SOC as argument.  
'climatisationStopped' - Climatisation.  
'climatisationStarted' - Climatisation started.  
'climatisationCoolingStarted' - Climatisation started cooling.  
'climatisationHeatingStarted' - Climatisation started heating.  
'climatisationTemperatureUpdated' - Target climatisation temperature changed.  

### Objects supplied by the API:

#### vwConn.vehicles - List of vehicles

#### vwConn.idData - Car data for the IDs

