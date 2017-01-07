```java
PauseTransition holder = new PauseTransition();
        holder.setDuration(Duration.seconds(1));
        imageView.setOnMousePressed(event1 -> {
            
            
            
            Point point = new Point((int) event1.getX(), (int) event1.getY());
            Point realPoint = ClickHelpUtil.getRealPoint(point);
            holder.setOnFinished(event2 -> {
                eventFlag[1] = true;
                if(isAll){
                    for (IDevice device : iDevices) {
                        ThreadPoolUtil.startLongMousePress(device,realPoint.getX(),realPoint.getY());
                    }
                }else {
                    ThreadPoolUtil.startLongMousePress(iDevice,realPoint.getX(),realPoint.getY());
                }
            });
            holder.playFromStart();
        });
```