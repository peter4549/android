# [Android lollipop shared elements transition blink / flash]()
```
Glide.with(context)
        .load(photoUriString)
        .centerCrop()
        // .diskCacheStrategy(DiskCacheStrategy.NONE)
        .error(R.drawable.ic_round_error_24)
        // .skipMemoryCache(true)
        // .transition(DrawableTransitionOptions.withCrossFade())
        .into(view.image_view)
```
