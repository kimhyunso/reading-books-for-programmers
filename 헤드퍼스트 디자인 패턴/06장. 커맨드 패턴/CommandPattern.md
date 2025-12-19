- 클라이언트에서 클래스가 무슨일을 하는지 모르도록 캡슐화가 가능하다

![image](./img/one.png)

### 코드

- command

```java
public interface Command{
		public void execute();
}

public class EmptyCommand implements Command{
		@Override
		public void execute(){
				
		}
}

public class LightOnCommand implements Command{
		private Light light;
		public LightOnCommand(Light light){
				this.light = light;
		}
		@Override
		public void execute(){
				light.on();
		}
}

public class LightOffCommand implements Command{
		private Light light;
		public LightOffCommand(Light light){
				this.light = light;
		}
		@Override
		public void execute(){
				light.off();
		}
}

public class GarageDownCommand implements Command{
		private Garage garage;
		public GarageDownCommand(Garage garage){
				this.garage = garage;
		}
		@Override
		public void execute(){
				garage.down();
		}
}

public class GarageUpCommand implements Command{
		private Garage garage;
		public GarageUpCommand(Garage garage){
				this.garage = garage;
		}
		@Override
		public void execute(){
				garage.up();
		}
}

...
```

- invoker

```java
public class RemoteControl{
		private Command[] onCommands;
		private Command[] offCommands;
		private int controlSize = 7;
		
		public RemoteControl(){
				Command emptyCommand = new EmptyCommand();
		
				for (int i=0; i<controlSize; i++){
						onCommands[i] = emptyCommand;
						offCommands[i] = emptyCommand;
				}
		}
		
		public void setCommand(int slot, Command onCommand, Command offCommand){
				onCommands[slot] = onCommand;
				offCommands[slot] = offCommand;
		}
		
		public void onButtonWasPushed(int slot){
				onCommands[slot].execute();
		}
		
		public void offButtonWasPushed(int slot){
				offCommands[slot].execute();
		}
		
		@Override
		public String toString(){
				StringBuffer stringBuff = new StringBuffer();
        stringBuff.append("\n----- 리모컨 -----\n");
        for (int i = 0; i < onCommands.length; i++){
            stringBuff.append("[slot " + i + "] " + onCommands[i].getClass().getName()
             + "   " + offCommands[i].getClass().getName() + "\n");
        }
        return stringBuff.toString();
		}
}
```

- receiver

```java
public class Light{
		private String location;
		
		public Light(String location){
				this.location = location;
		}
		
		public void on(){
				System.out.println(location + " is off");
		}
		public void off(){
				System.out.println(location + " is on");
		}
}

public class Garage{
		private String location;
		
		public Garage(String location){
				this.location = location;
		}
		
		public void down(){
				System.out.println(location + " shutter is down");
		}
		
		public void up(){
				System.out.println(location + " shutter is up");
		}
}
...
```

- client

```java
public class RemoteControlLoader{
		public static void main(String[] args){
				RemoteControl remoteControl = new RemoteControl();
				Light light = new Light("Living Room");
				Garage garage = new Garage("Living Room");
				
				LightOnCommand lightOn = new LightOnCommand(light);
				LightOffCommand lightOff = new LightOffCommand(light);
				
				GarageDownCommand garageDown = new GarageDownCommand(garage);
				GarageUpCommand garageUp = new GarageUpCommand(garage);
				
				remoteControl.setCommand(0, lightOn, lightOff);
				remoteControl.setCommand(1, garageUp, garageDown);
				
				System.out.println(remoteControl);
				// 거실 조명 켜기
				remoteControl.onButtonWasPushed(0);
				// 거실 조명 끄기
				remoteControl.offButtonWasPushed(0);
				// 차고지 올리기
				remoteControl.onButtonWasPushed(1);
				// 차고지 내리기
				remoteControl.offButtonWasPushed(1);
				
		}
}
```