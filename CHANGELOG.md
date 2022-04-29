
# Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## [0.3.4] - 2022-04-28

- Fix optional geometry and bbox fields on `Feature`; allowing users to pass in `None` or even omit either field (author @moradology, https://github.com/developmentseed/geojson-pydantic/pull/56)
- Fix `Polygon.from_bounds` to respect geojson specification and return counterclockwise linear ring (author @jmfee-usgs, https://github.com/developmentseed/geojson-pydantic/pull/49)

## [0.3.3] - 2022-03-04

- Follow geojson specification and make feature geometry optional (author @yellowcap, https://github.com/developmentseed/geojson-pydantic/pull/47)
    ```python
    from geojson_pydantic import Feature
    # Before
    feature = Feature(type="Feature", geometry=None, properties={})

    >> ValidationError: 1 validation error for Feature
    geometry none is not an allowed value (type=type_error.none.not_allowed)

    # Now
    feature = Feature(type="Feature", geometry=None, properties={})
    assert feature.geometry is None
    ```

## [0.3.2] - 2022-02-21

- fix `parse_geometry_obj` potential bug (author @geospatial-jeff, https://github.com/developmentseed/geojson-pydantic/pull/45)
- improve type definition (and validation) for geometry coordinate arrays (author @geospatial-jeff, https://github.com/developmentseed/geojson-pydantic/pull/44)

## [0.3.1] - 2021-08-04

### Added
- `Polygon.from_bounds` class method to create a Polygon geometry from a bounding box.
    ```python
    from geojson_pydantic import Polygon
    print(Polygon.from_bounds((1, 2, 3, 4)).dict(exclude_none=True))
    >> {'coordinates': [[(1.0, 2.0), (1.0, 4.0), (3.0, 4.0), (3.0, 2.0), (1.0, 2.0)]], 'type': 'Polygon'}
    ```

### Fixed
- Added validation for Polygons with zero size.


## [0.3.0] - 2021-05-25

### Added
- `Feature` and `FeatureCollection` model generics to support custom geometry and/or properties validation (author @iwpnd, https://github.com/developmentseed/geojson-pydantic/pull/29)

    ```python
    from pydantic import BaseModel
    from geojson_pydantic.features import Feature
    from geojson_pydantic.geometries import Polygon

    class MyFeatureProperties(BaseModel):
        name: str
        value: int

    feature = Feature[Polygon, MyFeatureProperties](
        **{
            "type": "Feature",
            "geometry": {
                "type": "Polygon",
                "coordinates": [
                [
                    [13.38272,52.46385],
                    [13.42786,52.46385],
                    [13.42786,52.48445],
                    [13.38272,52.48445],
                    [13.38272,52.46385]
                ]
                ]
            },
            "properties": {
                "name": "test",
                "value": 1
            }
        }
    )
    ```

- Top level export (https://github.com/developmentseed/geojson-pydantic/pull/34)

    ```python
    # before
    from geojson_pydantic.features import Feature, FeatureCollection
    from geojson_pydantic.geometries import Polygon

    # now
    from geojson_pydantic import Feature, Polygon
    ```

### Removed
- Drop python 3.6 support
- Renamed `utils.py` to `types.py`
- Removed `Coordinate` type in `geojson_pydantic.features` (replaced by `Position`)

## [0.2.3] - 2021-05-05

### Fixed
- incorrect version number set in `__init__.py`

## [0.2.2] - 2020-12-29

### Added
- Made collections iterable (#12)
- Added `parse_geometry_obj` function (#9)

## [0.2.1] - 2020-08-07

Although the type file was added in `0.2.0` it wasn't included in the distributed package. Use this version `0.2.1` for type annotations.

### Fixed
- Correct package type information files

## [0.2.0] - 2020-08-06

### Added
- Added documentation on locally running tests (#3)
- Publish type information (#6)

### Changed
- Removed geojson dependency (#4)

### Fixed
- Include MultiPoint as a valid geometry for a Feature (#1)

## [0.1.0] - 2020-05-21

### Added
- Initial Release
