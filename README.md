#2015-12-21


##1.12-21 18:49:44.927  20426-20426/sabria.demo.tinybus D/Event﹕ No subscribers registered for event class de.greenrobot.event.NoSubscriberEvent

### Describe:

MainActivity post an event to SecondActivity :

    findViewById(R.id.gotoSecond).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                startActivity(new Intent(MainActivity.this, SecondActivity.class));
                EventBus.getDefault().post(new ProcessDataEvent(20));
            }
        });
        
        
        
        
 
### How To solve

    findViewById(R.id.gotoSecond).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                startActivity(new Intent(MainActivity.this, SecondActivity.class));
                /**
                 12-21 18:49:44.927  20426-20426/sabria.demo.tinybus D/Event﹕ No subscribers registered for event class sabria.demo.tinybus.event.ProcessDataEvent
                 12-21 18:49:44.927  20426-20426/sabria.demo.tinybus D/Event﹕ No subscribers registered for event class de.greenrobot.event.NoSubscriberEvent
                 */
                new Handler().postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        EventBus.getDefault().post(new ProcessDataEvent(20));
                    }
                }, 50);

            }
        });
        
        
### WHY

因为SecondActivity此时还没有生成,所以会报NoSubscriberEvent,延迟50毫秒post就可以解决问题.




##2.de.greenrobot.event.EventBusException: Subscriber class com.xxx.xxx.MainActivity has no public methods called onEvent

产生问题的解决方案有两个方面:
1.该MainActivity中你使用了

    EventBus.getDefault().register(this);
    
但是没有类似onEventMainThread()方法;


2.onEventMainThread这样的方法的参数你要么没有要么就是错误的.

解决方案很简单:

EventBus post()方法的类是不需要EventBus.getDefault().register(this)这样的说明的,它和otto不太相同.

同时注意:onEventMainThread()的参数

    public void onEventMainThread(ProcessDataEvent data) {
        String msg = "onEventMainThread收到了消息：" +data.getTotalSteps();
        Toast.makeText(this, msg, Toast.LENGTH_LONG).show();
    }

















