public class Main {
        import edu.wpi.first.wpilibj.TimedRobot;
        import edu.wpi.first.wpilibj.drive.DifferentialDrive;
        import edu.wpi.first.wpilibj.smartdashboard.SendableChooser;
        import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
        import edu.wpi.first.wpilibj.Joystick;
        import edu.wpi.first.wpilibj.Timer;
        import edu.wpi.first.wpilibj.motorcontrol.Spark;
     public class Robot extends TimedRobot {
     private static final String kDefaultAuto = "Default";
     private static final String kCustomAuto = "My Auto";
     private String m_autoSelected;
     private final SendableChooser<String> m_chooser = new SendableChooser<>();
    
        private Spark leftMotor;
        private Spark rightMotor;
        private DifferentialDrive robotDrive;
        private Joystick joystick;
        @Override
        public void robotInit() {
        m_chooser.setDefaultOption("Default Auto", kDefaultAuto);
        m_chooser.addOption("My Auto", kCustomAuto);
        SmartDashboard.putData("Auto choices", m_chooser);
    
        leftMotor = new Spark(0);  // Motor izquierdo en PWM 0
        rightMotor = new Spark(1); // Motor derecho en PWM 1
    
        robotDrive = new DifferentialDrive(leftMotor, rightMotor);
        joystick = new Joystick(0);
        }
        @Override
        public void autonomousInit() {
        m_autoSelected = m_chooser.getSelected();
        System.out.println("Auto selected: " + m_autoSelected);
        }
    
        @Override
        public void autonomousPeriodic() {
        switch (m_autoSelected) {
          case kCustomAuto:
            performCustomAuto();
            break;
          case kDefaultAuto:
          default:
            performDefaultAuto();
            break;
        }
        }
        private void performDefaultAuto() {
        moveForward(0.5, 2.0);
        turnRight(0.5, 1.0);
     }
    
        private void performCustomAuto() {
        moveForward(0.7, 3.0);
        turnLeft(0.5, 1.5);
     }
    
    private void moveForward(double speed, double time) {
        robotDrive.tankDrive(speed, speed);
        Timer.delay(time);
        robotDrive.stopMotor();
     }
    
        private void turnRight(double speed, double time) {
        robotDrive.tankDrive(speed, -speed);
        Timer.delay(time);
        robotDrive.stopMotor();
        }
    
        private void turnLeft(double speed, double time) {
        robotDrive.tankDrive(-speed, speed);
        Timer.delay(time);
        robotDrive.stopMotor();
        }
        @Override
        public void teleopPeriodic() {
        double speed = joystick.getY();
        double rotation = joystick.getX();
        robotDrive.arcadeDrive(speed, rotation);
        }
        @Override
        public void disabledInit() {}
        @Override
        public void disabledPeriodic() {}
        @Override
        public void testInit() {}
        @Override
        public void testPeriodic() {}
        @Override
        public void simulationInit() {}
        @Override
        public void simulationPeriodic() {}
    }                    
}
