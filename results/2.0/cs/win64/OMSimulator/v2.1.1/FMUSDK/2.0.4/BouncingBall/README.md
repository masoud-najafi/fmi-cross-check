To simulate FMU `BouncingBall.fmu` with OMSimulator run
```bash
$ wine64 /fmi-cross-check/OMSimulatorBinaries/OMSimulator-mingw64/bin/OMSimulator.exe --stripRoot=true --skipCSVHeader=true --addParametersToCSV=true --intervals=500 --suppressPath=true --timeout=60 BouncingBall.lua
```

Lua file:
```lua
-- Lua file for BouncingBall.fmu
oms_setTempDirectory("temp")
oms_newModel("model")
oms_addSystem("model.root", oms_system_wc)

-- instantiate FMU
oms_addSubModel("model.root.fmu", "../../../../../../../../../fmus/2.0/cs/win64/FMUSDK/2.0.4/BouncingBall/BouncingBall.fmu")

-- Simulation settings
oms_setSignalFilter("model", ".*")
oms_setResultFile("model", "BouncingBall_out.csv")
oms_setStartTime("model", 0.0)
oms_setStopTime("model", 4.0)
oms_setTolerance("model", 0.01)
initialStepSize, minimumStepSize, maximumStepSize, status = oms_getVariableStepSize("model")
oms_setVariableStepSize("model", 0.01, minimumStepSize, 0.01)
oms_setFixedStepSize("model", 0.01)

-- Instantiate, initialize and simulate
oms_instantiate("model")
oms_initialize("model")
oms_simulate("model")
oms_terminate("model")
oms_delete("model")
```

See the [OMSimulator documentation](https://openmodelica.org/doc/OMSimulator/master/html/index.html) for more information.