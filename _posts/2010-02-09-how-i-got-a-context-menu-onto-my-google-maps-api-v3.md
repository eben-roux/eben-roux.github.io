---
layout: post
title: "How I got a context menu onto my Google Maps API V3"
date: 2010-02-09 00:34:00
comments: true
categories: 
---

I have a <a href="http://www.jquery.com">jquery</a> context menu plug-in that I have been using for a while. As things go the context menu is attached to the relevant element (my map <span style="font-family: courier new,courier;">div</span>) like so:

<span style="font-size: x-small;"><span style="font-family: courier new,courier;">$('#map-canvas').contextMenu('context-menu', _{options});</span></span>

This is all good-and-well except that when the map is rendered all kinds of magic happens that basically means my context menu is never displayed. This is in all likelihood because the Google Maps populates the map canvas div with elements that obvious _do not have my context menu attached.

So the approach I took was to somehow tell the context menu to pop up where I need it. But then I couldn't find a context menu that had a simple <span style="font-family: courier new,courier;">Show</span> or <span style="font-family: courier new,courier;">Display</span> or <span style="font-family: courier new,courier;">PopUp</span> method (maybe I just missed it). So the next step was to trigger the 'contexmenu' event on the <span style="font-family: courier new,courier;">map-canvas</span> div.

First I needed to add an event listener for the <span style="font-family: courier new,courier;">rightclick</span> event on the map that would call the <span style="font-family: courier new,courier;">openContextMenu</span> function. However, the event object passed to the function by Google Maps is a <span style="font-family: courier new,courier;">MouseEvent</span> class. This only has a <span style="font-family: courier new,courier;">latLng</span> property specifying the latitude and longitude. From this I needed to get the XY pixel coordinates. Fortunately there is a <span style="font-family: courier new,courier;">MapCanvasProjection</span> object that can return a <span style="font-family: courier new,courier;">Point</span> from the <code>fromLatLngToContainerPixel </code>method. Unfortunately the <span style="font-family: courier new,courier;">MapCanvasProjection</span> object cannot be returned from the map but rather from an overlay. So after some googling I found a bit of code on StackOverflow that does just that.

If anyone has a more elegant way to achieve the same thing please let me know.

Here is the complete code:

<span style="font-family: courier new,courier;">var map;var mapOverlay;var contextMenuEvent;$(document).ready(function() { var sw = new google.maps.LatLng(-34.80, 16.43); var ne = new google.maps.LatLng(-22.06, 32.75); var bounds = new google.maps.LatLngBounds(sw, ne); map = new google.maps.Map($("#map-canvas").get(0), { mapTypeId: google.maps.MapTypeId.ROADMAP }); map.setCenter(bounds.getCenter()); map.fitBounds(bounds); map.enableKeyDragZoom(); mapOverlay = new MapOverlay(map); google.maps.event.addListener(map, "rightclick", openContextMenu); $.contextMenu.defaults( { menuStyle: { width: '200px' } }); $('#map-canvas').contextMenu('context-menu', { bindings: { 'context-menu-zoom-in': function(trigger) { map.setZoom(map.getZoom() + 1); }, 'context-menu-zoom-out': function(trigger) { map.setZoom(map.getZoom() - 1); }, 'context-menu-center-map': function(trigger) { map.setCenter(contextMenuEvent.latLng); } } });});function openContextMenu(e) { var ev = new jQuery.Event('contextmenu'); var p = mapOverlay.getProjection().fromLatLngToContainerPixel(e.latLng); ev.pageX = p.x; ev.pageY = p.y;  contextMenuEvent = e; $('#map-canvas').trigger(ev, [e.latLng]);}MapOverlay.prototype = new google.maps.OverlayView();MapOverlay.prototype.onAdd = function() { }MapOverlay.prototype.onRemove = function() { }MapOverlay.prototype.draw = function() { }function MapOverlay(map) { this.setMap(map); }</span>
