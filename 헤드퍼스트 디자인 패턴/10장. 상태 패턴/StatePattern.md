- 상태를 객체로 캡슐화하여 저장한다.

### 코드
- 매진상태
        
```java
public class SoldOutState implements State{
        private GumballMachine gumballMachine;

        public SoldOutState(GumballMachine gumballMachine){
                this.gumballMachine = gumballMachine;
        }
        
        @Override
        public void insertCoin(){
                System.out.println("매진 상태입니다.");
        }
        
        @Override
        public void ejectCoin(){
                System.out.println("반환할 수 있는 동전이 없습니다.");
        }
        
        @Override
        public void turnCoin(){
                System.out.println("매진 상태라 손잡이를 돌릴 수 없습니다.");
        
        }
        
        @Override
        public void dispense(){
                System.out.println("알맹이를 내보낼 수 없습니다.");
        }
}
```
    
- 코인이 없는 상태
    
```java
public class NoCoinState implements State{

}
```
    
- 코인이 있는 상태
    
```java
public class HasCoinState implements State{

}
```
    
- 판매상태
    
```java
public class SoldState implements State{
        private GumballMachine gumballMachine;
        
        public SoldState(GumballMachine gumballMachine){
        
        }
        
        @Override
        public void insertCoin(){
        
        }
        
        @Override
        public void ejectCoin(){
        
        }
        @Override
        public void turnCoin(){
        
        }
        
        @Override
        public void dispense(){
        
        }
}
```
    

```java
public interface State{
        // 코인 넣기
        public void insertCoin();
        // 코인 빼기
        public void ejectCoin();
        // 머신 돌리기
        public void turnCoin();
        // 알맹이 추출
        public void dispense();
}

public class GumballMachine{
        private State hasCoinState;
        private State noCoinState;
        private State soldState;
        private State soldOutState;
        
        private State state = soldOutState;
        private int count;
        
        public GumballMachine(int numberGumballs){
                hasCoinState = new HasCoinState(this);
                noCoinState = new NoCoinState(this);
                soldState = new SoldState(this);
                soldOutState = new SoldOutState(this);
                // 알맹이 재고
                this.count = numberGumballs;
        if (numberGumballs > 0){
            state = noCoinState;
        }else{
            state = soldOutState;
        }
        }
        
        public void insertCoin(){
                state.insertCoin();
        }
        
        public void ejectCoin(){
                state.ejectCoin();
        }
        
        public void turnCoin(){
                state.turnCoin();
                state.dispense();
        }
        
        public void releaseBall(){
        System.out.println("알맹이를 내보내고 있습니다.");
        if (count > 0){
            count -= 1;
        }
    }
    // Getter Setter
    ...
}
```