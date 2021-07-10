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
