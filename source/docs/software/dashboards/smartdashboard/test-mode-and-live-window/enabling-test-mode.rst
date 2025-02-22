Enabling Test mode (LiveWindow)
===============================

You may add code to your program to display values for your sensors and actuators while the robot is in Test mode. This can be selected from the Driver Station whenever the robot is not on the field (see :ref:`docs/software/basic-programming/using-test-mode:Enabling Test Mode` for details on how to do this). Test mode is designed to verify the correct operation of the sensors and actuators on a robot. In addition it can be used for obtaining setpoints from sensors such as potentiometers and for tuning PID loops in your code.  When the robot is enabled in Test mode, the SmartDashboard display will switch to test mode (LiveWindow) and will display the status of any actuators and sensors used by your program.


.. important:: Since 2024, LiveWindow is not enabled by default in Test mode!

Enabling LiveWindow in Test Mode
--------------------------------

To run LiveWindow in Test Mode, the following code is needed in the ``Robot`` class:

.. tab-set-code::

    .. code-block:: java

        @Override
        public void robotInit() {
          enableLiveWindowInTest(true);
        }

    .. code-block:: c++

        void Robot::RobotInit() {
          EnableLiveWindowInTest(true);
        }


Explicitly vs. implicit test mode display
-----------------------------------------

.. tab-set-code::

    .. code-block:: java

        PWMSparkMax leftDrive;
        PWMSparkMax rightDrive;
        PWMVictorSPX arm;
        BuiltInAccelerometer accel;

        @Override
        public void robotInit() {
          leftDrive = new PWMSparkMax(0);
          rightDrive = new PWMSparkMax(1);
          arm = new PWMVictorSPX(2);
          accel = new BuiltInAccelerometer();
          SendableRegistry.setName(arm, "SomeSubsystem", "Arm");
          SendableRegistry.setName(accel, "SomeSubsystem", "Accelerometer");
        }

    .. code-block:: c++

        frc::PWMSparkMax leftDrive{0};
        frc::PWMSparkMax rigthDrive{1};
        frc::BuiltInAccelerometer accel{};
        frc::PWMVictorSPX arm{3};

        void Robot::RobotInit() {
          wpi::SendableRegistry::SetName(&arm, "SomeSubsystem", "Arm");
          wpi::SendableRegistry::SetName(&accel, "SomeSubsystem", "Accelerometer");
        }

All sensors and actuators will automatically be displayed on the SmartDashboard in test mode and will be named using the object type (such as PWMSparkMax, PWMVictorSPX, BuiltInAccelerometer, etc.) with channel number with which the object was created. In addition, the program can explicitly add sensors and actuators to the test mode display, in which case programmer-defined subsystem and object names can be specified making the program clearer. This example illustrates explicitly defining those sensors and actuators.

Understanding what is displayed in Test mode
--------------------------------------------

.. image:: images/enabling-test-mode/test-mode-display.png
   :alt: Highlights both ungrouped and subsystem motors displayed in test mode.

This is the output in the SmartDashboard display when the robot is placed into test mode. In the display shown above the objects listed as Ungrouped were implicitly created by WPILib when the corresponding objects were created. These objects are contained in a subsystem group called "Ungrouped" **(1)** and are named with the device type (PWMSparkMax in this case), and the channel numbers. The objects shown in the "SomeSubsystem" **(2)** group are explicitly created by the programmer from the code example in the previous section. These are named in the calls to ``SendableRegistry.setName()``. Explicitly created sensors and actuators will be grouped by the specified subsystem.
