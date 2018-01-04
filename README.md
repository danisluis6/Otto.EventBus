- @link https://github.com/square/otto
> Analyze: 
   - LocationActivity (**Activity**)
   - LocationMapFragment(**Fragment**)
   - LocationHistoryFragment(**Fragment**)
   
> Implement Code: 
   - Definite Instance to get Bus(Object)
            
            public final class BusProvider {
            
              private static Bus mInstance;
            
              public static Bus getInstance() {
                if (mInstance == null) {
                  mInstance = new Bus();
                }
                return mInstance;
              }
            
              private BusProvider() {
                // No instances.
              }
            }
            
   - Implement in LocationMainActivity
   
           @Override protected void onResume() {
               super.onResume();
           
               // Register ourselves so that we can provide the initial value.
               BusProvider.getInstance().register(this);
           }
           
   - Update the location in History Fragment
            
           findViewById(R.id.clear_location).setOnClickListener(new OnClickListener() {
             @Override public void onClick(View v) {
               // Tell everyone to clear their location history.
               BusProvider.getInstance().post(new LocationClearEvent());
       
               // Post new location event for the default location.
               lastLatitude = DEFAULT_LAT;
               lastLongitude = DEFAULT_LON;
               BusProvider.getInstance().post(produceLocationEvent());
             }
           });
           
   -        