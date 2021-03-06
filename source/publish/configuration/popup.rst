.. _popup:

Popup
=====

Activate popup
---------------

With the plugin, you can activate popups **for a single layer** or for **a group configured with the "Group as layer" option**.

Just click on the checkbox **Activate popups** of the tab *Layers* on the Lizmap plugin interface. For the *Group as layer* option you must select the option for the group and for all the layers included you want to show in the popup: in this case, only the layers with the option *Popup* checked will be shown.

You have three types of popup sources:

* *auto*
* *lizmap*
* *qgis*

In the web application Lizmap Web Client, a click on a map object will trigger the popup if (and only if):

* the layer is **active in the legend**, so that it is shown on the canvas
* the popup **has been activated** through the plugin for the layer or the group
* the user has clicked on an **area of the canvas** where data for the layer with active popups are displayed.

.. note:: For point layers you need to click in the middle of the point to display the popup. The tolerance can be setup in tab *Map options* then **Map tools**.

You can update where the popup is displayed in the web interface in *Map options* then **Map interface**. You can choose between:

* *dock*
* *minidock*
* *map*
* *bottomdock*
* *right-dock*


Auto popup
-----------

The Lizmap Web Client `auto` popup displays a table showing the columns of the attribute table in two columns *Field* and *Value*, as shown below:

============  ==============
Field         Value
============  ==============
          id  1
        name  A name
 description  This object ...
       photo  :-)
============  ==============

You can modify the info displayed through QGIS, and also display pictures or links.

Simple popup configuration
____________________________

With the plugin, if you click on the checkbox **Activate popups** without modifying its content through the button *Configure* the default table is shown.

Nevertheless, you can tune several things in QGIS and with the help of Lizmap plugin to **parametrize the fields displayed**, **rename fields**, and even **display images, photos, or links to internal or external documents**.

Mask or rename a column
_______________________

You can use the tools available in the **Fields** tab of the **Layer properties**, in QGIS:

* to **avoid displaying** a column in the popup, **uncheck the relative WMS checkbox**. The WMS column is on the right

* to **change the name** displayed for that column, type a different name in the *Alias* column

.. image:: /images/features-popup-fields.jpg
   :align: center
   :width: 70%

Usage of media: images, documents, etc.
_______________________________________

If you use **paths to documents of the media directory**, you can:

* *display the image* found at that link
* *display the content (text or HTML)* of the file
* *display a link* to a document

.. seealso:: Chapter :ref:`media` for more details on the usage of documents of the directory media in the popups.

Usage of external links
_______________________

You can also use, in a field, **full web links to a specific page or image**:

* the image referred to will be displayed, instead of the links
* the web link will be displayed and clickable

Lizmap popup
------------

Introduction
____________

If the simple table display does not suit your needs, you can write a **popup template**. To do so, you should know well the **HTML format**. See e.g.: https://html.net/tutorials/html/

.. warning:: When you use the *lizmap* mode, the previous configuration to rename a field does not work anymore: you have to configure what is displayed and how through the template. Managing media is also possible, but you have to configure it as well.

Deploying
_________

You can edit the popup template with the button *Configure* in the Lizmap plugin. Clicking on it you'll get a window with two text areas:

* an **area where you can type your text**
* a **read-only area**, showing a preview of your template

.. image:: /images/features-popup-configure.jpg
   :align: center
   :width: 70%

You can type simple text, but we suggest to write in HTML format to give proper formatting. For instance, you can add paragraphs, headings, etc.:

.. code-block:: html

   <h3>A Title</h3>
   <p>An example of paragraph</p>

The behaviour is as follows:

* if the content of the two areas is empty, a simple table will be shown in the popup (default template)
* if the content is not empty, its content will be used as a template for the popup

Lizmap Web Client will replace automatically a variable, identified by the name of a field, with its content. To add the content of a column to a popup, you should use the name of the column precede by a dollar sign (`$`), all surrounded by curly brackets (`{}`). For instance:

.. code-block:: html

   <h3>A Title</h3>
   <p>An example of paragraph</p>
   <p>A name: <b>{$name}</b></p>
   <p>Description: {$description}</p>

.. note:: If you have configured an alias for a field, you have to use the alias instead of the name, between the brackets.

You can also use the values of the columns as parameters to give styling to the text. An example here, to use the colour of a bus line as a background colour:

.. code-block:: html

   <p style="background-color:{$color}">
   <b>LINE</b> : {$ref} - {$name}
   <p/>

Usage of media and external links
_________________________________

You can **use the media** referred to in the table content, even if you use a *template model*. To do this, you should use the media column, taking into account the fact that Lizmap Web Client automatically replaces the relative path of the type ``/media/myfile.jpg`` with the full URL to the file, accessible through the web interface.

You can also use full URLs pointing to the pages or images on another server.

Here an example of a template handling media and an external link:

.. code-block:: html

   <p style="font-size:0.8em;">A Title</p>
   <p>The name is {$name}</p>
  <p>
     A sample image<br/>
     <img src="{$image_column}" style="">
   </p>

   <p><a href="{$website}" target="_blank">Web link</a></p>

   <p><img src="https://www.3liz.com/images/logo-lizmap.png"/></p>

.. seealso:: Chapter :ref:`media` for more details on the use of documents in the directory media.

QGIS popup
-----------

*QGIS* popups can be configured via `QGIS --> Layer properties --> Tooltips --> HTML`, using the same syntax as for the *lizmap* popups. The main advantages of this approach are:

* you can use QGIS variables and expressions, thus adding information created dynamically
* the popup can be previewed in QGIS, using tooltips
* the popup configurations are stored in QGIS project and layer style, so they can be reused in other Lizmap projects without replicating the configuration.

One to many relations
---------------------

It is possible to display multiple objects (photos, documents) for each geographical feaature. To do so, you have to configure both the QGIS project and the Lizmap config.

In QGIS project:

* Use 2 separate layers to store the main features and the pictures. For example "trees" and "tree_pictures". The child layer must contain a field referencing the parent layer id
* Configure aliases and field types in tab Fields of the layers properties dialog. Use "Photo" for the field which will contains the relative path to pictures
* Add a relation in QGIS project properties between the main layer "trees" and the child layer "tree_pictures"
* Add data to the layers. You should use relative path to store the pictures path. Theses paths must refer to a project media subdirectory, for example: media/photos/feature_1_a.jpg

In Lizmap plugin:

* In the Layers tab, activate popup for both layers. You can configure popup if you need specific layouts ( See documentation on popups )
* For the parent layer, activate the option "Display relative children under each object (use relations)"
* Add the two layers in the Attribute table tab
* You can optionally activate editing for the two layers, to allow the web users to create new features and upload pictures
* Save and publish your project and Lizmap configuration

Link of an element for layers with an atlas
___________________________________________

Every element of a layer with an atlas configured will have a link at the end of his popup which open a pdf of the atlas for this particular element.
To make it work you will need to download the "atlas_print" plugin, for that you have to get it from his Github at : https://github.com/3liz/qgis-atlasprint

Display children in a compact way
_________________________________

You can change the way children are displayed and make them look like a table. For that, you will need to adapt the HTML of your children layer and use a few classes to manipulate it.

* "lizmap_merged" : You need to attribute this class to your table
* lizmapPopupHeader : If you want to have a better display of your headers, you will need to put this class in the '<tr>' who contains them
* lizmapPopupHidden : This class permit you to hide some elements of your children that you want to hide when there are used as a child but you still want to see them if you display their popup as a main Popup

Here an example:

.. code-block:: html

 <table class="lizmap_merged">
  <tr class="lizmapPopupHeader">
      <th class="lizmapPopupHidden"><center> Idu </center></th>
      <th> <center> Type </center> </th>
      <th> <center> Surface</center> </th>
   </tr>
   <tr>
      <td class="lizmapPopupHidden"><center>[% "idu" %]</center></td>
      <td><center>[% "typezone" %]</center></td>
      <td><center>[% "surface" %]</center></td>
   </tr>
 </table>

.. image:: /images/popup_display_children.jpg
   :align: center
   :width: 80%
