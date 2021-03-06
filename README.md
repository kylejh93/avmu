
# AVMU Interface

Python 3 interface to AKELA Inc's vector meaurement units.

Quickstart:

    import avmu

    AVMU_IP_ADDRESS = "192.168.1.219"
    HOP_RATE        = "HOP_15K"
    START_F         = 250
    STOP_F          = 2100
    NUM_POINTS      = 1024
    SWEEP_COUNT     = 100

    device = avmu.AvmuInterface()
    device.setIPAddress(AVMU_IP_ADDRESS)
    device.setIPPort(1027)
    device.setTimeout(500)
    device.setMeasurementType("PROG_ASYNC")

    device.initialize()

    device.setHopRate(HOP_RATE)
    device.addPathToMeasure('AVMU_TX_PATH_0', 'AVMU_RX_PATH_1')

    device.utilGenerateLinearSweep(startF_mhz=START_F, stopF_mhz=STOP_F, points=NUM_POINTS)

    # Get the freqency plan that utilGenerateLinearSweep calculated given the
    # hardware constraints.
    frequencies = device.getFrequencies()

    # Arm the device
    device.start()

    sweeps = []

    # Tell the AVMU to start asynchronous acquisitions.
    device.beginAsync()

    # Consume asynchronously generated frequency sweeps
    for _ in range(count):

        device.measure()
        sweep_data = device.extractAllPaths()
        sweeps.append(sweep_data)
        print("Acquired sweep (%s)" % (len(sweeps), ))

    # Stop the asynchronous acquisition
    device.haltAsync()

    # Finally, disarm the acquisition.
    device.stop()


Significantly more comprehensive examples are included in `demo-simple.py` and `demo-threaded.py`.

 - `demo-simple.py` shows how to properly convert acquired frequency domain data into time-domain 
   data, for extracting meaningful range profiles. It also includes a waterfall plot for viewing
   motion the returned data, if desired.
 - `demo-threaded.py` implements a much more robust, error tolerant client for the AVMU, with proper
   handling for the various way the network connection can hiccup.

### Usage:

If you clone this repository, the examples can be run directly, as the `avmu` package is present 
in the repository as well. However, for external projects, this depends on you manually placing
the `avmu` directory in the root of any project that would like to use it.

Alternatively, `avmu` is also available in PyPi, so `pip install avmu` will make the `avmu`
interface package available globally. At that point, either of the example files can be run
from any location.









