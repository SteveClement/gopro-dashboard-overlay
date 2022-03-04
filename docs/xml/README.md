## Dashboard Configuration

The dashboard is configured by an XML file. There are built-in dashboards, and these use the XML configuration. Have a
look at the [example file](../../gopro_overlay/layouts/example.xml) to see an example, or see the
[default layout](../../gopro_overlay/layouts/default-1920x1080.xml)

### Tiny Example

The main element is `layout`: everything is contained within these tags.

```xml

<layout>
    <translate name="bob" x="200" y="200">
        <component type="text" x="0" y="0" size="32" cache="False">Bob</component>
        <translate x="180">
            <component type="compass" size="300" fg="255,255,255" text="0,255,255" textsize="32"/>
        </translate>
    </translate>
</layout>
```

## Containers

Some components can contain other components, these are called "containers".

### Translate Component

The `translate` component moves its child elements around on the screen. For example, the `compass` component doesn't
take an x/y co-ordinate to indicate where it should be drawn on the screen, so by default it will draw at (0,0) - the
top-left of the screen. By placing the `compass` inside a `translate` the compass can be drawn anywhere on the screen

```xml

<translate x="180" y="50">
    <component type="compass" size="300" fg="255,255,255" text="0,255,255" textsize="32"/>
</translate>
```

### Frame Component

A `frame` is a box that can contain other components. It can optionally be filled with a background colour, and have an
outline drawn around it. The corners of the box can be rounded with `cr` and opacity with `opacity`.

Any child can be placed inside a `frame` - much like a `composite` - however, child components will be clipped to the
size of the frame. This could be used to create some interesting effects. A square `frame` with `cr` set to half the
width (i.e. the radius) will create a circular frame!

```xml

<frame width="200" height="200" cr="100" opacity="0.5" outline="255,255,255">
    <!-- child components ... -->
</frame>
```

### Composite Component

This works identically to the `translate` component

### Naming Containers

Optionally, any container component can be named, this allows you to include or exclude it for a given rendering run on
the command line Names don't have to be unique, so a dashboard could have a number of different containers all named "
with-hr", which could be excluded when rendering a GPX track that doesn't have any heartrate data in it.

```xml

<translate name="with-hr" x="180" y="50">
    <component type="text">HR</component>
    <component type="metric" x="-70" y="18" metric="hr" dp="0" size="32" align="right"/>
</translate>
```

If you didn't want to render the metric and the text, you could add `--exclude with-hr` when running the program, and
this container would be skipped.

## Components

Components are simple widgets that draw something onto the screen.

[Text](#text-component), [Metric](#metric-component), [Datetime](#datetime)
[Moving Map](#moving-map), [Journey Map](#journey-map)
[Icon](#icon)

### Moving Map

Shows a moving map, with the current GPS location at the centre of the map. The zoom level can be set to show a smaller
or larger surrounding area. Zoom levels range from 0 (the whole world) to 20 (a mid-sized building) - although not all
map privders will provide tiles for zoom levels 19 & 20. Zoom levels 18 and below should be widely supported.

For more information on zoom levels see: [Zoom Levels](https://wiki.openstreetmap.org/wiki/Zoom_levels) on the
OpenStreetMap wiki.

Example:

```xml

<component type="moving_map" name="some-name" x="1644" y="100" size="256" zoom="16" corner_radius="35"/>
```

### Journey Map

Shows an 'overall' journey map, with the entire GPS trace shown over a map. The map is scaled appropriately to fit the
entire journey.

```xml

<component type="journey_map" name="some-name" x="1644" y="376" size="256" corner_radius="35"/>
```

### Gradient Chart

This component will be made more generic in a future version to chart any metric.

```xml
<component type="gradient_chart" name="some-name" x="400" y="980"/>
```
### Metric Component


##### Metric Units

Metrics all are associated with a given unit. For example, speed is always in `m/s`. Metrics can be converted to
different units using the `units` attribute.

```xml

<component type="metric" metric="temp" units="degreeF" dp="1"/>
```

The following units are supported: `mph`, `kph`, `mps`, `knots`, `degreeF`, `degreeC`, `feet`, `miles`, `nautical_miles`, `radian`

Conversions that don't make sense for a given metric will fail with a suitable message.


