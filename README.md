# practice-code

```python

import astropy

# here is my python code
name = 'Aakash'
for i in reversed(range(5)):
  print("hello {}".format(name)
 ```
 
 
 Code documentation because I'm too dumb to write a for loop

Java example

```java

package com.team1678.frc2020.subsystems;

import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.can.TalonFX;
import com.team254.lib.drivers.TalonFXFactory;
import com.team254.lib.drivers.MotorChecker;
import com.team254.lib.drivers.TalonFXChecker;

import com.team254.lib.util.TimeDelayedBoolean;

import com.team1678.frc2020.Constants;
import com.team1678.frc2020.loops.ILooper;
import com.team1678.frc2020.loops.Loop;

import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.DigitalInput;
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;

// import com.team1678.frc2020.subsystems.Shooter;

import java.util.ArrayList;

public class Conveyor extends Subsystem {

    private static Conveyor mInstance;

    // for proxy slots: { 1 (bottom), 2, 3, 4 (top) }
    private static final boolean[] kEmptyProxySlots = {false, false, false, false};
    private static final boolean[] kFullProxySlots = {true, true, true, true};
    private static int kTopProxySlot = 1;
    private static boolean full_proxy_slots = false;

    private TimeDelayedBoolean mProxyTriggered = new TimeDelayedBoolean();
    private static double kTriggeredTime = 0.1; // check value

    private TimeDelayedBoolean mProxySlotsChecker = new TimeDelayedBoolean();
    private static double kProxySlotsCheckTime = 1.0; // check value

    private static double kIdleVoltage = 0.0;
    private static double kCollectingVoltage = 5.0; // TODO: Determine best voltage
    private static double kIndexingVoltage = 3.0; // TODO: Determine best voltage
    private static double kFeedingVoltage = 7.0; // TODO: Determine best voltage
    private static double kSpittingVoltage = -10.0;

    private static final double kJamCurrent = 150.0;
    private double mLastCurrentSpikeTime = 0.0;
    private static final double kCurrentIgnoreTime = 1.0;

    public enum WantedAction {
        NONE, COLLECT, INDEX, FEED, SPIT
    }

    public enum State {
        IDLE, COLLECTING, INDEXING, FEEDING, SPITTING
    }

    private State mState = State.IDLE;

    private static PeriodicIO mPeriodicIO = new PeriodicIO();

    private final TalonFX mHoriz;
    private final TalonFX mVertical;

    private DigitalInput mProxy1 = new DigitalInput(Constants.kProxy1);
    private DigitalInput mProxy2 = new DigitalInput(Constants.kProxy2);
    private DigitalInput mProxy3 = new DigitalInput(Constants.kProxy3);
    private DigitalInput mProxy4 = new DigitalInput(Constants.kProxy4);

    private Conveyor() {
        mHoriz = TalonFXFactory.createDefaultTalon(Constants.kHorizID);
        mVertical = TalonFXFactory.createDefaultTalon(Constants.kVerticalID);

        mHoriz.set(ControlMode.PercentOutput, 0);
        mHoriz.setInverted(false); // TODO: check
        mHoriz.configVoltageCompSaturation(12.0, Constants.kLongCANTimeoutMs);
        mHoriz.enableVoltageCompensation(true);

        mVertical.set(ControlMode.PercentOutput, 0);
        mVertical.setInverted(true); // TODO: check
        mVertical.configVoltageCompSaturation(12.0, Constants.kLongCANTimeoutMs);
        mVertical.enableVoltageCompensation(true);
    }

    public synchronized static Conveyor getInstance() {
        if (mInstance == null) {
            mInstance = new Conveyor();
        }
        return mInstance;
    }

    @Override
    public void outputTelemetry() {
        SmartDashboard.putNumber("Horizontal Belt Current", mPeriodicIO.collecting_current);
        SmartDashboard.putNumber("Vertical Belt Current", mPeriodicIO.indexing_current);

        SmartDashboard.putBoolean("Proxy 1", mPeriodicIO.raw_proxy_slots[0]);
        SmartDashboard.putBoolean("Proxy 2", mPeriodicIO.raw_proxy_slots[1]);
        SmartDashboard.putBoolean("Proxy 3", mPeriodicIO.raw_proxy_slots[2]);
        SmartDashboard.putBoolean("Proxy 4", mPeriodicIO.raw_proxy_slots[3]);
    }
    
```

# Please someone help me learn how to code
