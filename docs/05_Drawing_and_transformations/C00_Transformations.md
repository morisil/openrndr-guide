 
 # Transformations

This section covers the topic of placing items on the screen. 
 
 ## Basic transformation use 
 
 ### Translation
Translation moves points in space with an offset.

In the following example we use `Drawer.translate` to move a single rectangle over the screen. 
 
 <video controls>
    <source src="media/transformations-001.mp4" type="video/mp4"></source>
</video>
 
 
 ```kotlin
program {
    extend {
        drawer.fill = ColorRGBa.PINK
        drawer.stroke = null
        // set up translation
        drawer.translate(seconds * 100.0, height / 2.0)
        drawer.rectangle(-50.0, -50.0, 100.0, 100.0)
    }
}
``` 
 
 [Link to the full example](https://github.com/openrndr/openrndr-examples/blob/master/src/main/kotlin/examples/05_Drawing_and_transformations/C00_Transformations000.kt) 
 
 translations (and transformations in general) can be stacked on top of each-other. For example we can
express a horizontal and a vertical motion as two separate translations 
 
 <video controls>
    <source src="media/transformations-002.mp4" type="video/mp4"></source>
</video>
 
 
 ```kotlin
program {
    extend {
        drawer.fill = ColorRGBa.PINK
        drawer.stroke = null
        // move the object to the vertical center of the screen
        drawer.translate(0.0, height / 2.0)
        // set up horizontal translation
        drawer.translate(seconds * 100.0, 0.0)
        // set up vertical translation
        drawer.translate(0.0, cos(seconds * Math.PI * 2.0) * 50.00)
        
        drawer.rectangle(-50.0, -50.0, 100.0, 100.00)
    }
}
``` 
 
 [Link to the full example](https://github.com/openrndr/openrndr-examples/blob/master/src/main/kotlin/examples/05_Drawing_and_transformations/C00_Transformations001.kt) 
 
 ### Rotations 
 
 Rotating transformations are performed using `Drawer.rotate()`. The rotation is applied by rotating points
around the origin of the coordinate system: (0, 0), which lies in the top-left corner of the window.

In the first rotation example we rotate a rectangle that is placed around the origin but later translated to the center
of the screen. Here we notice something that may be counter-intuitive at first: the transformations are easiest read
from bottom to top: first `rotate` is applied and only then `translate`. 
 
 <video controls>
    <source src="media/transformations-003.mp4" type="video/mp4"></source>
</video>
 
 
 ```kotlin
program {
    extend {
        drawer.fill = ColorRGBa.PINK
        drawer.stroke = null
        
        // -- translate
        drawer.translate(width / 2.0, height / 2.0)
        // -- rotate
        drawer.rotate(seconds * 10.0)
        // -- rectangle around the origin
        drawer.rectangle(-50.0, -50.0, 100.0, 100.0)
    }
}
``` 
 
 [Link to the full example](https://github.com/openrndr/openrndr-examples/blob/master/src/main/kotlin/examples/05_Drawing_and_transformations/C00_Transformations002.kt) 
 
 ### Scaling 
 
 Scaling transformations are performed using `Drawer.scale()`. Also scaling is applied around the origin of
the coordinate system: (0, 0). 
 
 <video controls>
    <source src="media/transformations-004.mp4" type="video/mp4"></source>
</video>
 
 
 ```kotlin
program {
    extend {
        drawer.fill = ColorRGBa.PINK
        drawer.stroke = null
        
        // -- translate to center of screen
        drawer.translate(width / 2.0, height / 2.0)
        // -- scale around origin
        drawer.scale(cos(seconds * Math.PI * 2.0) + 2.0)
        // -- rectangle around the origin
        drawer.rectangle(-50.0, -50.0, 100.0, 100.00)
    }
}
``` 
 
 [Link to the full example](https://github.com/openrndr/openrndr-examples/blob/master/src/main/kotlin/examples/05_Drawing_and_transformations/C00_Transformations003.kt) 
 
 ### Combining transformations 
 
  
 
 <video controls>
    <source src="media/transformations-005.mp4" type="video/mp4"></source>
</video>
 
 
 ```kotlin
program {
    extend {
        drawer.fill = ColorRGBa.PINK
        drawer.stroke = null
        
        // -- translate to center of screen
        drawer.translate(width / 2.0, height / 2.0)
        
        drawer.rotate(20.00 + seconds * 10.0)
        // -- rectangle around the origin
        drawer.rectangle(-50.0, -50.0, 100.0, 100.0)
        

        // -- draw a second rectangle, sharing the rotation of the first rectangle but with an offset
        drawer.translate(150.0, 0.0)
        drawer.rectangle(-50.0, -50.0, 100.0, 100.0)
    }
}
``` 
 
 [Link to the full example](https://github.com/openrndr/openrndr-examples/blob/master/src/main/kotlin/examples/05_Drawing_and_transformations/C00_Transformations004.kt) 
 
 <video controls>
    <source src="media/transformations-006.mp4" type="video/mp4"></source>
</video>
 
 
 ```kotlin
program {
    extend {
        drawer.fill = ColorRGBa.PINK
        drawer.stroke = null
        
        // -- translate to center of screen
        drawer.translate(width / 2.0, height / 2.0)
        
        drawer.rotate(seconds * 10.0)
        // -- rectangle around the origin
        drawer.rectangle(-50.0, -50.0, 100.0, 100.0)
        
        // -- draw a second rectangle, sharing the rotation of the first rectangle but with an offset
        drawer.translate(150.0, 0.0)
        drawer.rotate(seconds * 20.0)
        drawer.rectangle(-50.0, -50.0, 100.0, 100.00)
    }
}
``` 
 
 [Link to the full example](https://github.com/openrndr/openrndr-examples/blob/master/src/main/kotlin/examples/05_Drawing_and_transformations/C00_Transformations005.kt) 
 
 ## Transform pipeline

OPENRNDR's `Drawer` is build around model-view-projection transform pipeline. That means that three different transformations are applied to determine
the screen position.

matrix property | pipeline stage
----------------|---------------------
`model`         | model transform
`view`          | view transform
`projection`    | projection transform

Which matrices are affected by which `Drawer` operations?

operation            | matrix property
---------------------|----------------
`fun rotate(…)`      | `model`
`fun translate(…)`   | `model`
`fun scale(…)`       | `model`
`fun rotate(…)`      | `model`
`fun lookAt(…)`      | `view`
`fun ortho(…)`       | `projection`
`fun perspective(…)` | `projection`


## Projection matrix

The default projection transformation is set to an orthographic projection using `ortho()`. The origin is in the upper-left corner; positive y points down, positive x points right on the screen.

### Perspective projections

```kotlin
override fun draw() {
    drawer.perspective(90.0, width*1.0 / height, 0.1, 100.0)
}
```

## Transforms

In OPENRNDR transforms are represented by `Matrix44` instances.

OPENRNDR offers tools to construct `Matrix44`

### Transform builder

Relevant APIs
```
Matrix44
transform {}
```

In the snippet below a `Matrix44` instance is constructed using the `transform {}` builder. Note that the application order is from bottom to top.

```kotlin
drawer.view *= transform {
    rotate(32.0)
    rotate(Vector3(1.0, 1.0, 0.0).normalized, 43.0)
    translate(4.0, 2.0)
    scale(2.0)
}
```

This is equivalent to the following:
```kotlin
drawer.rotate(32.0)
drawer.rotate(Vector3(1.0, 1.0, 0.0).normalized, 43.0)
drawer.translate(4.0, 2.0)
drawer.scale(2.0)
```

## Applying transforms to vectors ##

```kotlin
    val x = Vector3(1.0, 2.0, 3.0)
    val m = transform {
        rotate(42.0)
    }
    val transformed = m * x
    val transformedTwice = m * m * x
```
 
