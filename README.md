# learning-rxKotlin
Cook book for RXKotlin


## Creating an Observable
```
var stringy = "Hi"

        var x: Observable<String> = Observable.create(ObservableOnSubscribe<String> { emitter ->
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
