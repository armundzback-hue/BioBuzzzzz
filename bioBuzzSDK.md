```java
package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;

@TeleOp(name = "BIOBUZZ TeleOp", group = "Main")
public class BioBuzzTeleOp extends LinearOpMode {


    DcMotor frontLeft;
    DcMotor frontRight;
    DcMotor backLeft;
    DcMotor backRight;

    DcMotor intake;
    DcMotor launcher;

    @Override
    public void runOpMode() {

        frontLeft = hardwareMap.get(DcMotor.class, "frontLeft");//sa schimb trag in stefan
        frontRight = hardwareMap.get(DcMotor.class, "frontRight");
        backLeft = hardwareMap.get(DcMotor.class, "backLeft");
        backRight = hardwareMap.get(DcMotor.class, "backRight");

        intake = hardwareMap.get(DcMotor.class, "intake");
        launcher = hardwareMap.get(DcMotor.class, "launcher");

        frontLeft.setDirection(DcMotor.Direction.REVERSE);
        backLeft.setDirection(DcMotor.Direction.REVERSE);

        telemetry.addLine("BIOBUZZ ready");
        telemetry.update();

        waitForStart();

        while (opModeIsActive()) {


            double y = -gamepad1.left_stick_y;
            double x = gamepad1.left_stick_x;
            double rx = gamepad1.right_stick_x;

            double frontLeftPower  = y + x + rx;
            double backLeftPower   = y - x + rx;
            double frontRightPower = y - x - rx;
            double backRightPower  = y + x - rx;

            double max = Math.max(
                    1.0,
                    Math.max(
                            Math.abs(frontLeftPower),
                            Math.max(
                                    Math.abs(backLeftPower),
                                    Math.max(
                                            Math.abs(frontRightPower),
                                            Math.abs(backRightPower)
                                    )
                            )
                    )
            );

            frontLeftPower /= max;
            backLeftPower /= max;
            frontRightPower /= max;
            backRightPower /= max;

            frontLeft.setPower(frontLeftPower);
            backLeft.setPower(backLeftPower);
            frontRight.setPower(frontRightPower);
            backRight.setPower(backRightPower);
            if (gamepad1.right_bumper) {
                intake.setPower(1.0);
            }
            else if (gamepad1.left_bumper) {
                intake.setPower(-1.0);
            }
            else {
                intake.setPower(0);
            }
            double launcherPower = gamepad1.right_trigger;

            launcher.setPower(launcherPower);

            telemetry.addData("Intake", intake.getPower());
            telemetry.addData("Launcher", launcher.getPower());

            telemetry.update();
        }
    }
}
```