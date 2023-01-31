```java
package GoedertierTimo;
import robocode.*;

import robocode.HitRobotEvent;
import robocode.Robot;
import robocode.ScannedRobotEvent;
import robocode.WinEvent;
import static robocode.util.Utils.normalRelativeAngleDegrees;


 //GTonFire - a robot by Timo Goedertier

public class GTonFire extends JuniorRobot
{
	int moveCounter = 0;
    int moveDirection = 1;
    int moveDistance = 100;

    public void run() {
        	//Robot parts: body, gun, radar, bullet, scan_arc
		setColors(0x000000, 0xF100FF, 0xF1FF00, 0xFF0092, 0x00FFF2);
		
        while (true) {
			turnGunRight(360);
				//Different kinds of Behavior
			int checkPlayers = others;
			if ( checkPlayers > 20 || energy < 20) {
				defense();	//Behavior1
				} 
			else if ( checkPlayers < 20) {
				balanced(); //Behavior 2
				}
			else if ( checkPlayers < 10) {
				attack(); //Behavior 3
				}
			else if (checkPlayers == 0){
				onWin(); //If this robot is the last one standing
				}
        }
    }
		//onScannedRobot: What to do when you scan an enemy Robot
    public void onScannedRobot() {
		int scannedDist = scannedDistance;
        turnGunTo(scannedAngle);
			//Different fire power based on enemy distance
        if (scannedDistance < 100) {
            fire(2);
			}
		else if (scannedDistance > 100 && scannedDistance < 200){
			fire(1.5);
			}
		else if (scannedDistance > 200 && scannedDistance < 400){
			fire(1);
			}
		else if (scannedDistance > 400){
			fire(0.5);
			}
    }

   		//onHitByBullet: What to do when you're hit by a bullet
	public void onHitByBullet() {
		turnGunTo(hitByBulletAngle);
		fire(1);
		turnBackLeft(70, 45 - hitByBulletBearing);
		back(75);
	}
	
		//onHitByRobot: What to do when you're hit by another robot
	public void onHitByRobot() {
	turnGunTo(hitRobotAngle);
	fire(3);
	back(150);
	}

   		//onHitWall: What to do when you touch the wall
	public void onHitWall() {
		turnTo(hitWallAngle);
		turnRight(180);
		ahead(150);
		ahead(200);
	}	
		//Behavior 1
	public void defense() {
        double movement = fieldHeight - robotY;
		int head = heading;
        if (head < 90) {
            ahead((int)movement);
            turnRight(90 - head);
        } else if (head > 90) {
            back((int)movement);
            turnLeft(head - 90);
        } else {
            ahead((int)movement);
        }
    }
		//Behvaior 2
	public void balanced() {
        if (moveCounter % 20 == 0) {
            moveDirection *= -1;
        }
        ahead(moveDistance * moveDirection);
        turnRight(90);
        moveCounter++;
        moveDistance += 5;
    }
		//Behavior 3
	public void attack() {
		int scannedDist = scannedDistance;
		int scannedBear = scannedBearing;
		if (scannedDist > 0) {
           int angle = scannedBear + heading - gunHeading;
           bearGunTo(angle);
           ahead(scannedDist - 50);
           turnRight(scannedBear);
         }
         else {
            turnGunRight(360);
            ahead(fieldHeight/2);
         }
	}
	
		//onWin do a 'Vicotry Dance'
	public void onWin() {
		for (int i = 0; i < 50; i++) {
			turnRight(30);
			turnLeft(30);
		}
	}
}
```
