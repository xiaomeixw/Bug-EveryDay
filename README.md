##1.12-21 18:49:44.927  20426-20426/sabria.demo.tinybus D/Event﹕ No subscribers registered for event class de.greenrobot.event.NoSubscriberEvent

### Describe:

MainActivity post SecondActivity an event:

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









