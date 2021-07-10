# learning-rxKotlin
Cook book for RXKotlin


## Creating a single Observable

If you have a List of items that you want use you can use a for each loop in the `onNext()` or you can use `just()` which acomodates for collection.

```
var stringy = "Hi"

        var x: Observable<String> = Observable.create(ObservableOnSubscribe<String> { emitter ->  //Lambda
            if(!emitter.isDisposed){
                emitter.onNext(stringy)
                emitter.onComplete()
            }

        })
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())

        x.subscribe(object : Observer<String> {
            override fun onSubscribe(d: Disposable?) {

            }

            override fun onNext(t: String?) {
                Log.d(TAG, "onNext: $t")
            }

            override fun onError(e: Throwable?) {
                Log.d(TAG, "onError: ${e.toString()}")
            }

            override fun onComplete() {
                Log.d(TAG, "Complete")
            }

        })
        
```

## Creating an instance of retro fit 
### This allows us to convert a retrofit Call object to a Observable or a Flowable
```
 retrofit = Retrofit.Builder()
                .baseUrl(API_BASE_URL)
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create()) 
                .addConverterFactory(GsonConverterFactory.create())
                .build()
```


## Creating our Json place holder API
### Retrofit fills out the methods in the interface for us

```
jsonWeatherAPI = retrofit.create(JsonWeatherAPI::class.java)
```

## API Endpoint manager
```
@GET("/api/location/{whereOnEarthId}")
    fun getWeatherForWhereOnEarthId(@Path("whereOnEarthId") whereOnEarthId: Int): Flowable<WeatherForLocation>
```

## Making the call and setting our Views

```
jsonWeatherAPI.getWeatherForWhereOnEarthId(44418)
                .toObservable()
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe { weatherForLocation ->
                    cityTextView.text = weatherForLocation.cityTitle
                    stateTextView.text = weatherForLocation.parentRegion.title
                    temperatureTextView.text = weatherForLocation.consolidatedWeather[0].theTemp.toString()
                    pressureValueTextView.text = weatherForLocation.consolidatedWeather[0].airPressure.toString()
                    humidityValueTextView.text = weatherForLocation.consolidatedWeather[0].humidity.toString()
                    windSpeedValueTextView.text = weatherForLocation.consolidatedWeather[0].windSpeed.toString()
                }
```
