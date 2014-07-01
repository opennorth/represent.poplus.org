---
layout: inner-page
title: API Reference
---

## Basics

### Endpoints

The API has two top-level endpoints:

* [Boundary sets](#boundary-sets)
* [Boundaries](#boundaries)

All endpoints output JSON.

### Pagination

Results are paginated 20 per page by default. Set the number of results per page by adding a `limit` query parameter. Change pages using the `offset` query parameter or using the `next` and `previous` links under the `meta` field in the response to navigate to the next and previous pages (if any). Under the `meta` field, `total_count` is the number of results. For example:

```json
{
  "objects": [
    ...
  ],
  "meta": {
    "total_count": 300,
    "next": "/boundaries/?limit=20&offset=20",
    "offset": 0,
    "limit": 20,
    "previous": null
  }
}
```

### Filtering results

Filter results with query parameters. Each endpoint below lists the fields on which you can filter results. To filter for boundaries with the name “Toronto”, for example, request `/boundaries/?name=Toronto`.

Perform substring searches by appending `__querytype` to the parameter name, where `querytype` is one of:

* `iexact`
* `contains` or `icontains`
* `startswith` or `istartswith`
* `endswith` or `iendswith`
* `isnull`

A leading `i` makes the search case-insensitive. For example, to find boundaries with a name starting with “M” or “m”, request `/boundaries/?name__istartswith=m`.

### Debugging

For a browsable, HTML version of the JSON response, add a `format=apibrowser` query parameter. Add `pretty=1` to just indent the raw JSON.

### JSONP

JSONP is supported for client-side cross-domain requests – just add a `callback` query parameter.

<h2 id="boundary-sets">Boundary sets</h2>

A boundary set is a group of boundaries.

* Get one page of boundary sets' summaries

        /boundary-sets/

* Get one boundary set's details

        /boundary-sets/federal-electoral-districts/

* Filter boundary sets by `name` or `domain`

        /boundary-sets/?domain=Canada

To see responses to these API requests, try them on [http://represent.opennorth.ca](http://represent.opennorth.ca/).

<h2 id="boundaries">Boundaries</h2>

* Get one page of boundaries' summaries

        /boundaries/

* Get one page of boundaries from a boundary set

        /boundaries/toronto-wards/

* Get one page of boundaries from multiple boundary sets (comma-separated)

        /boundaries/?sets=toronto-wards,ottawa-wards

* Get one boundary's details

        /boundaries/toronto-wards/etobicoke-north-1/

* Filter boundaries by `name` or `external_id`

        /boundaries/?name=Toronto

To see responses to these API requests, try them on [http://represent.opennorth.ca](http://represent.opennorth.ca/).

### Geospatial queries

* Find boundaries by latitude and longitude

        /boundaries/?contains=45.524,-73.596

* Find boundaries that touch

        /boundaries/?touches=toronto-wards/etobicoke-north-1

* Find boundaries that intersect (&ldquo;covers or overlaps&rdquo; in PostGIS lingo)

        /boundaries/?intersects=toronto-wards/etobicoke-north-1

To see responses to these API requests, try them on [http://represent.opennorth.ca](http://represent.opennorth.ca/).

### Drawing boundaries

The default geospatial output format is GeoJSON. Add a `format=kml` or `format=wkt` query parameter to get KML or Well-Known Text.

The `simple_shape` endpoint simplifies the shape, looks fine and loads fast. Represent Boundaries uses a tolerance of `0.0002` by default to simplify the shape. You can change the tolerance by setting `SIMPLE_SHAPE_TOLERANCE` in `my_project/settings.py`. If you change the tolerance, run `loadshapefiles` with the `--reload` switch.

* Get all simple shapes from a boundary set

        /boundaries/toronto-wards/simple_shape

* Get all original shapes from a boundary set

        /boundaries/toronto-wards/shape

* Get all centroids from a boundary set

        /boundaries/toronto-wards/centroid

* Get one boundary's simple shape

        /boundaries/toronto-wards/etobicoke-north-1/simple_shape

* Get one boundary's original shape

        /boundaries/toronto-wards/etobicoke-north-1/shape

* Get one boundary's centroid

        /boundaries/toronto-wards/etobicoke-north-1/centroid

To see responses to these API requests, try them on [http://represent.opennorth.ca](http://represent.opennorth.ca/).