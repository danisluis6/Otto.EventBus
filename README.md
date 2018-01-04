- @link http://www.vogella.com/tutorials/JavaLibrary-EventBusOtto/article.html#ottoeventbus_whenouse
#### Installation 
- Using Maven or Gradle as build system you can simply add a dependency to it.

        dependencies {
          compile 'com.squareup:otto:1.3.8'
        }
        
#### When to use Otto?
- Otto can be used to communicate between your activity and fragments or to communicate between an activity and a service.
#### How to set up Otto?
- To use Otto, create a singleton instance of the Bus class and provide access to it for your Android components. This is typically done in the Application object of your Android application.

        public static Bus bus = new Bus(ThreadEnforcer.MAIN);
        
- In this example the ThreadEnforcer.MAIN parameter is used,. This enforces Otto to send events always from the main thread. If you want to be able to send events from any thread use the ThreadEnforcer.ANY parameter.

#### How to register and unregister for events?
- Event registration is done via the @Subcribe annotation on a public single parameter method. The method parameter is the event key, i.e., if such an data type is send via the Otto event bus the method is called.
- Event receivers must register via the register method of the Bus class.
#### How to register and unregister for events?
- Subscribe for string messages/TestData
    - Event registration is done via the @Subcribe annotation on a public single parameter method. The method parameter is the event key, i.e., if such an data type is send via the Otto event bus the method is called.
    - Event receivers must register via the register method of the Bus class.
    
            // subscribe for string messages
            
            @Subscribe
            public void getMessage(String s) {
                Toast.makeText(this, s, Toast.LENGTH_LONG).show();
            }
            
            //subscribe for TestData messages
            
            @Subscribe
            public void getMessage(TestData data) {
                Toast.makeText(getActivity(), data.message, Toast.LENGTH_LONG).show();
            }
            
            //requires a registration e.g. in the onCreate method
            bus.register(this);

    - To unregister from events use the unregister() method.
#### How to send events
- For sending events it is not necessary to register with the event bus. Simple call the post() method of the `Bus class.

        // post a string object
        bus.post("Hello");
        
        // example data to post
        public class TestData {
            public String message;
        }
        
        // post this data
        bus.post(new TestData().message="Hello from the activity");
        
#### How can new components receive the last event?
- Sometimes new components, like a dynamically created fragment, should receive event data during their creation. If this case component can register as producer for such event data with the @Produce annotation.
- Event receivers must register via the register method of the Bus class.

        @Produce
        public String produceEvent() {
            return "Starting up";
        }