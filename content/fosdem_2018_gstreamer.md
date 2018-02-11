Title: GStreamer Fosdem 2018
Date: 2018-02-11 16:30:00
Modified: 2018-02-11 16:30:00
Category: FOSDEM 
Tags: FOSDEM, OpenSource, gstreamer
Slug: fosdem-2018-gstreamer
Authors: Michiel Jordens
Summary: GStreamer related talks on FOSDEM 2018

# [Gstreamer for embedded devices](https://fosdem.org/2018/schedule/event/om_gst_embedded/)

GStreamer is widespread. It's used by TVs (LG, Samsung), 
settopboxes, In-flight-entertainment, the space station
and the list goes on.

##Some new features that are now supported:

- Better zero-copy support (tee)
- Stablized v4l2 names
- hardware codecs
- Changing decoder resolutions at runtime

##KMS improvements
*KMS is the linux driver that puts the pixels on your screen*

- DMAbuf pool
- video-overlay
- more formats and devices

##Embedded OpenGL:

- Vivante EGL FB support (increases performance on imx6)
- Moved into base and frozen API
- Mesa DMAbuf export support

##Process seperation:
*This allows putting a sink in a different process and
use the source as a slave*

- Master/Slave model
- Useful for terrible APIs that only work with high privileges

##Near future:
- DRM modifiers
- GStreamer CI on embedded systems
- V4L2 stateless codecs


# [Gstreamer for tiny devices](https://fosdem.org/2018/schedule/event/gstreamer_for_tiny_devices/)

This was a talk trying to get gstreamer to compile and 
run on a device with a very small footprint. 

There were 3 things that he did :

- Link statically with libtool
- Butcher glib (even with compile options certain
sections were still compiled in the library)
- Compress the binary with upx

To link statically he had to use :

- GST_PLUGIN_STATIC_DECLARE
- GST_PLUGIN_STATIC_REGISTER
- GST_DEBUG=GST_PLUGIN_LOADING:4

An interesting tool he used is called 
[bloaty mcbloatface](https://github.com/google/bloaty).

# [Modern tools to debug GStreamer applications](https://fosdem.org/2018/schedule/event/om_gst_dbg/)

## GstTracer
- available from 1.8
- Traces modules loaded at runtime
- Post-run analysis and live introspection
- Monitoring hooks
- Formatted output

## Stats Tracers
- GST_TRACER="stats,usage"
- GST_DEBUG=GST_TRACER
- gst-stats \*.log

## Latency Tracers
This can measure the time it takes for each buffer
to travel from source to sink.
GST_TRACERS=latency

## Leaks Tracer
* Tracks refcounts of GObjefct and GstMiniObject, Only track leaks in gst code.
* Raises warning on leaks.
* Integrated in 1.10.
* No false positives
* Lighter/faster than valgrind.
* Hook into CI / QA system

## Tracing leaks
* GST_TRACER=leaks 
* GST_TRACER=stack-trace
* Track ref/unref operations (check-refs=true)
* List alive objects while running (SIGUSR1)
* Check points (SIGUSR2)

You can use GST_TRACER=leaks to get a high level overview.
If you see one object that has leaks, you can use the
GST_TRACER=stack-trace to really zoom in on that object by
visualizing the stack trace.

## [GSTShark](https://github.com/RidgeRun/gst-shark)
* Inter latency
* buffer rate on src pad
* schedule time
* Queues level

GSTShark can show which element is underperforming.
It supports creating graphs which are quite nice in showing
where the bottlenecks are. This is not part of gstreamer.
These are a few plugin scripts you have to install seperately

## [gst-log-parser](https://github.com/gdesmott/gst-log-parser)
* GStreamer logs parsing library
* High level objects to manipulate logs
* Easy filtering, mapping, etc (iterator)
* ts-diff : highlight highest timestamps gaps
