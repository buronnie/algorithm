问题：如何设计一个停车场？

可以做如下假设：
1. 停车场有多层，每层有多行停车位

2. 停车场可以停摩托车，小轿车和公交车

3. 停车场有摩托车位，紧凑型车位，和大型车位

4. 摩托车可以停在任何位置

5. 小轿车可以停在紧凑型车位和大型车位

6. 公交车只能停在同一行中连续的五个大型车位上，不能停在小位置上

```java
enum VehicleSize { Large, Compact, Motocyle };
class Vehicle {
    public int spotsNum() {};
    public VehicleSize size() {};
    protected List<ParkingSpot> parkingSpots = new ArrayList<>();
    public park(ParkingSpot spot) {
        parkingSpots.add(spot);
    }
    public clearSpots() {
        for (int i= 0; i < parkingSpots.size(); i++) {
            parkingSpots.get(i).removeVehicle();
        }
        parkingSpots.clear();
    }
}
class Bus extends Vehicle {
    public spotsNum() {
        return 5;
    }
    public VehicleSize size() {
        return VehicleSize.Large;
    }
}
class ParkingSpot {
    public ParkingSpot(Level level, int r, int n, VehicleSize vs) {
        level = lv;
        row = r;
        spotNumber = n;
        spotSize = sz;
    }
    pubic boolean isAvailable() {
        return vehicle == null;
    }
    public boolean canFitVehicle(Vehicle vehicle) {
        return isAvailable() && spotSize >= vehicle.size();
    }

    public boolean park(Vehicle v) {
        if (!canFitVehicle(v)) {
            return false;
        }
        vehicle = v;
        vehicle.park(this);
        return true;
    }

    public removeVehicle() {
        level.spotFreed();
        vehicle = null;
    }
}
class Level {
    private ParkingSpot[] spots;
    private int fFloor;
    private int fAvailableSpots;
    private int fNumberOfRows;
    private int fSpotsPerRow;

    public Level(int floor, int numberOfRows, int spotsPerRow) {
        fFloor = floor;
        fNumberOfRows = numberOfRows;
        fSpotsPerRow = spotsPerRow;

        int[] motoSpots = new Spot();
    }
    public parkVehicle（Vehicle vehicle) {
        int vehicleSize = vehicle.size();
        if (vehicleSize === VehicleSize.Large) {
            for (int i = 0; i < motoSpots.size(); i++) {
                if (motoSpots[i] == null) {
                    motoSpots[i] = vehicle;
                    vehicle.park(this, i);
                }
            }
        }
    }
}
```