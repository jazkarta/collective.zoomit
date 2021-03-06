collective.zoomit
=================

This add-on integrates the `zoom.it <http://zoom.it/>`_ service into
Plone.  It includes a Dexterity behavior which can be assigned to
content types, and also a marker interface ``IZoomItImage`` which
can be assigned to individual instances or to content classes (using
e.g. ``implements``, ``classImplements``, or the zcml ``class``
directive).

Zoom.it is a Microsoft Live Labs provided webservice which generates
DeepZoom tiled image representations of hosted images and provides a
friendly SeaDragon-based viewer for them.  It allows very high
resolution images to be viewed in a friendly manner.

The adapter/behavior provided in this package assumes the content it
is applied to has an ``image`` attribute or a ``getImage`` method. The
package provides scripts to replace the image in the primary view with
the js/silverlight viewer.  That script assumes that the primary view
of the content includes the primary image in the main content area
inside of an anchor (i.e. ``#content a > img:first-child``).

The zoom.it API is called to process the image the first time the
content is saved.  However, the API call will only be made if the
image is publicly visible (because the zoom.it service needs to
retrieve it via a public url).  If the image has not yet been
processed, Image processing will also be initiated after workflow
transitions.  As a result, image content that was not publicly
accessible to the zoom.it service will be automatically processed once
it becomes accessible via a workflow transition.  This will not cause
non-workflowed images in private folders to be processed when the
folder is made public.  In that case, the image must be resaved or
manually processed using the Zoom.it action menu.

Processing of large images may take some time (and may occasionally
fail), to handle this the add-on provides an action menu item for
viewing and updating the progress of the image processing and manually
re-initiating the process after a failure.  If the processing has not
been completed, editing the content will also update the status
information.

Editing the image field will also cause the image to be re-processed.


Caveats
-------

Microsoft's Zoom.it service is an unsupported experimental service,
with questionable reliability.  It is also a one of a kind way to make
it easy to display very high resolution images with minimal effort via
a convenient API.  Images which have already been processed seem to be
reliably available from the service; however, there appear to be
extended periods where the service will not accept new images.  As a
result, initial processing may fail.  For that reason this add-on is
designed as a progressive enhancement, and will only display the
viewer when the processed image is available.

See the `FAQ <http://zoom.it/pages/faq/>`_.

Because Zoom.it requires a publicly accessible URL to retrieve the
image, this add-on will not work on ``private`` content or when edited
from non-internet accessible urls.  The adapter intentionally skips
requests originating from loopback addresses, but a placeholder image
can be used for testing from a local address by setting the variable
``collective.zoomit.config.DEBUG`` to ``True``.


Contributors
------------

* `Jazkarta, Inc. <http://www.jazkarta.com>`_


    * Alec Mitchell



Thanks To
---------

* `Dumbarton Oaks Garden Archives <http://doaks.org>`_
