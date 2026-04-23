# Geometry Library Migration from AutoDrive

This is a direct migration of the 2D geometry components from the AutoDrive project, preserving the original structure and namespace.

## Migrated Components

### Core Vector Library (`geom/common/`)
- `vec.h` - N-dimensional vector template with specializations for 2D and 3D
- `check.h` - Check macros using glog
- `common.h` - Common utilities and constants
- `math_util.h` - Mathematical utilities
- `proto/vec.pb.h` - Simplified protobuf interface for Vec2d

### 2D Geometry (`geom/2d/`)
- `segment_2.h` - 2D line segment class
- `polyline_2d.h/.cpp` - 2D polyline with arc-length parameterization

### Legacy Geometry (`geom/`)
- `geometry_legacy.h` - Axis-aligned bounding box (AABox2)

## Namespace Structure

- `geom` - Main geometry namespace (Vec, Segment2, Polyline2d)
- `geom_legacy` - Legacy namespace (AABox2)

## File Structure
```
geom2d/
├── geom/
│   ├── common/
│   │   ├── check.h
│   │   ├── common.h
│   │   ├── math_util.h
│   │   ├── vec.h
│   │   └── proto/
│   │       └── vec.pb.h
│   ├── 2d/
│   │   ├── segment_2.h
│   │   ├── polyline_2d.h
│   │   └── polyline_2d.cpp
│   └── geometry_legacy.h
├── BUILD
├── WORKSPACE
└── README.md
```

## Key Changes from Original

1. **Cross-platform compatibility**: Changed `sprintf_s` to `snprintf` in `common.h`
2. **Simplified protobuf**: Created a simplified `vec.pb.h` instead of generated protobuf
3. **Build system**: Added Bazel build files

## Usage Example

```cpp
#include "geom/common/vec.h"
#include "geom/2d/segment_2.h"
#include "geom/2d/polyline_2d.h"
#include "geom/geometry_legacy.h"

using namespace geom;

// Vector operations
Vec2d a(1.0, 2.0);
Vec2d b(3.0, 4.0);
Vec2d sum = a + b;

// Segment operations
Segment2d seg(Vec2d(0.0, 0.0), Vec2d(10.0, 0.0));
double distance = seg.DistanceTo(Vec2d(5.0, 3.0));

// Polyline operations
std::vector<Vec2d> points = {Vec2d(0.0, 0.0), Vec2d(3.0, 0.0), Vec2d(3.0, 4.0)};
Polyline2d polyline(points);
Vec2d point_at_s = polyline.PointAtS(2.5);

// AABox operations (legacy namespace)
geom_legacy::AABox2d box(1.0, 3.0, 2.0, 4.0);
double width = box.XSpan();
```

## Building with Bazel

```bash
# Build the library
bazel build //:geom

# Use as dependency in other projects
cc_library(
    name = "my_lib",
    deps = ["@geom2d//:geom"],
)
```

## Dependencies

- C++17 or later
- Google Logging (glog)

## Notes

This is a faithful migration preserving the original AutoDrive structure. The code maintains the same interfaces and behavior as the original.