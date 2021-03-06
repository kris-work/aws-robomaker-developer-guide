# JSON Schema for Simulation World Template Body<a name="worlds-creating-json-schema"></a>

The `templateBody` \(simulation world template body\) is an input parameter of the [https://docs.aws.amazon.com/robomaker/latest/dg/API_CreateWorldTemplate.html](https://docs.aws.amazon.com/robomaker/latest/dg/API_CreateWorldTemplate.html)\. This parameter is a JSON\-formatted string\. The JSON specifies a simulation world template and contains the parameters Simulation WorldForge will use to generate worlds\. 

The following is the schema for the JSON\. 

```
    {
  "title": "WorldTemplate",
  "description": "The top-level template for parameterizing a randomly generated world. By default, a single\nresidential building with one floor and one room is generated.",
  "type": "object",
  "properties": {
    "Version": {
      "title": "Version",
      "default": "1",
      "type": "string"
    },
    "Buildings": {
      "title": "Buildings",
      "default": [
        {
          "Floors": [
            {
              "Floorplan": {
                "Footprint": {
                  "DesiredAspectRatio": {
                    "x": 1.0,
                    "y": 1.0
                  }
                },
                "Ceiling": {
                  "Height": 3.0
                },
                "Rooms": [
                  {
                    "Type": "Living",
                    "Name": "My Living Room",
                    "DesiredShape": {
                      "Area": 20.0,
                      "AspectRatio": {
                        "x": 1.0,
                        "y": 1.0
                      }
                    }
                  },
                  {
                    "Type": "Bedroom",
                    "Name": "My Bedroom",
                    "DesiredShape": {
                      "Area": 20.0,
                      "AspectRatio": {
                        "x": 1.0,
                        "y": 1.0
                      }
                    }
                  }
                ],
                "DesiredConnections": []
              },
              "Interior": {
                "Flooring": {
                  "MaterialSets": []
                },
                "Walls": {
                  "MaterialSets": []
                },
                "Furniture": {
                  "FurnitureArrangements": []
                }
              }
            }
          ]
        }
      ],
      "type": "array",
      "items": {
        "$ref": "#/definitions/BuildingTemplate"
      },
      "minItems": 1,
      "maxItems": 1
    }
  },
  "additionalProperties": false,
  "definitions": {
    "AspectRatio": {
      "title": "AspectRatio",
      "type": "object",
      "properties": {
        "x": {
          "title": "X",
          "default": 1,
          "minimum": 1,
          "maximum": 4,
          "type": "number"
        },
        "y": {
          "title": "Y",
          "default": 1,
          "minimum": 1,
          "maximum": 4,
          "type": "number"
        }
      },
      "additionalProperties": false
    },
    "FloorplanFootprint": {
      "title": "FloorplanFootprint",
      "description": "The desired footprint of this floorplan.",
      "type": "object",
      "properties": {
        "DesiredAspectRatio": {
          "title": "Desiredaspectratio",
          "default": {
            "x": 1.0,
            "y": 1.0
          },
          "allOf": [
            {
              "$ref": "#/definitions/AspectRatio"
            }
          ]
        }
      },
      "additionalProperties": false
    },
    "FloorplanCeiling": {
      "title": "FloorplanCeiling",
      "description": "The height of the ceiling for this floorplan in metres.",
      "type": "object",
      "properties": {
        "Height": {
          "title": "Height",
          "default": 3.0,
          "type": "number",
          "minimum": 2.4,
          "maximum": 4.0
        }
      },
      "additionalProperties": false
    },
    "Rectangle": {
      "title": "Rectangle",
      "description": "A rectangle defined by area in square metres and aspect ratio.",
      "type": "object",
      "properties": {
        "Area": {
          "title": "Area",
          "type": "number"
        },
        "AspectRatio": {
          "$ref": "#/definitions/AspectRatio"
        }
      },
      "required": [
        "Area",
        "AspectRatio"
      ],
      "additionalProperties": false
    },
    "FloorplanRoom": {
      "title": "FloorplanRoom",
      "description": "A description for single room for this floorplan.",
      "type": "object",
      "properties": {
        "Type": {
          "title": "Type",
          "enum": [
            "Bedroom",
            "Bathroom",
            "Living",
            "Dining",
            "Kitchen",
            "Hallway",
            "Closet"
          ],
          "type": "string"
        },
        "Name": {
          "title": "Name",
          "type": "string"
        },
        "DesiredShape": {
          "title": "Desiredshape",
          "default": {
            "Area": 20.0,
            "AspectRatio": {
              "x": 1.0,
              "y": 1.0
            }
          },
          "allOf": [
            {
              "$ref": "#/definitions/Rectangle"
            }
          ]
        }
      },
      "required": [
        "Type",
        "Name"
      ],
      "additionalProperties": false
    },
    "FloorplanConnection": {
      "title": "FloorplanConnection",
      "description": "Descibes the desired layout of the rooms and their adjacent rooms. A connection can be either a doorway or \nan open space without any walls. Two rooms cannot both share an interior doorway and an opening. \nThe same two rooms can have multiple doorways, up to a limit.",
      "type": "object",
      "properties": {
        "Location": {
          "title": "Location",
          "type": "array",
          "items": {
            "type": "string"
          },
          "minItems": 2,
          "maxItems": 2
        },
        "ConnectionType": {
          "title": "Connectiontype",
          "enum": [
            "Doorway",
            "Opening"
          ],
          "type": "string"
        }
      },
      "required": [
        "Location",
        "ConnectionType"
      ],
      "additionalProperties": false
    },
    "FloorplanTemplate": {
      "title": "FloorplanTemplate",
      "description": "The top-level floorplan template that parameterizes the randomly generated \narchitectural layout. By default, a residential floorplan with bedroom and \nliving room are generated with a random doorway or opening connection. \n\nThe footprint contributes to the overall shape of the floor layout along\nwith rooms. The footprint shape is desired as it is a preference and not\nguaranteed.\n\nThe ceiling determines the height of the walls. There are minimum and\nmaximum ceiling heights. The ceiling height is guaranteed.\n\nRooms are required. Each room has a desired shape. Together, the room\nshapes and footprint determine floor layout. The room types contribute to\nthe layout and are used when randomly selecting furniture and materials for\nthe walls and floors.\n\nDesiredConnections are optional. Two rooms are connected if they share a\nwall and doorway or adjacent without any wall aka \"opening\". All rooms are\nguaranteed to be connected randomly if they are not specified in the\nconnections list. Connections that are specified are _not_ guaranteed but\nwill be attempted as best-effort.",
      "type": "object",
      "properties": {
        "Footprint": {
          "title": "Footprint",
          "default": {
            "DesiredAspectRatio": {
              "x": 1.0,
              "y": 1.0
            }
          },
          "allOf": [
            {
              "$ref": "#/definitions/FloorplanFootprint"
            }
          ]
        },
        "Ceiling": {
          "title": "Ceiling",
          "default": {
            "Height": 3.0
          },
          "allOf": [
            {
              "$ref": "#/definitions/FloorplanCeiling"
            }
          ]
        },
        "Rooms": {
          "title": "Rooms",
          "default": [
            {
              "Type": "Living",
              "Name": "My Living Room",
              "DesiredShape": {
                "Area": 20.0,
                "AspectRatio": {
                  "x": 1.0,
                  "y": 1.0
                }
              }
            },
            {
              "Type": "Bedroom",
              "Name": "My Bedroom",
              "DesiredShape": {
                "Area": 20.0,
                "AspectRatio": {
                  "x": 1.0,
                  "y": 1.0
                }
              }
            }
          ],
          "type": "array",
          "items": {
            "$ref": "#/definitions/FloorplanRoom"
          },
          "minItems": 1,
          "maxItems": 6
        },
        "DesiredConnections": {
          "title": "Desiredconnections",
          "default": [],
          "type": "array",
          "items": {
            "$ref": "#/definitions/FloorplanConnection"
          },
          "minItems": 0,
          "maxItems": 12
        }
      },
      "additionalProperties": false
    },
    "RoomNameList": {
      "title": "RoomNameList",
      "description": "The set of all rooms matching any of the listed room names.",
      "type": "object",
      "properties": {
        "RoomNames": {
          "title": "Roomnames",
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      },
      "required": [
        "RoomNames"
      ],
      "additionalProperties": false
    },
    "RoomTypeList": {
      "title": "RoomTypeList",
      "description": "The set of all rooms matching any of the listed room types.",
      "type": "object",
      "properties": {
        "RoomTypes": {
          "title": "Roomtypes",
          "type": "array",
          "items": {
            "enum": [
              "Bedroom",
              "Bathroom",
              "Living",
              "Dining",
              "Kitchen",
              "Hallway",
              "Closet"
            ],
            "type": "string"
          }
        }
      },
      "required": [
        "RoomTypes"
      ],
      "additionalProperties": false
    },
    "MaterialSetByMaterialType": {
      "title": "MaterialSetByMaterialType",
      "description": "The set of materials that match any of the material types listed.  An empty\nset is invalid since all targets require materials.",
      "type": "object",
      "properties": {
        "MaterialTypes": {
          "title": "Materialtypes",
          "type": "array",
          "items": {
            "type": "string"
          },
          "minItems": 1
        }
      },
      "required": [
        "MaterialTypes"
      ],
      "additionalProperties": false
    },
    "InteriorMaterialSet": {
      "title": "InteriorMaterialSet",
      "description": "A set of sample materials to randomly assign to a set of interior target elements.\n\nThe target set determines *what rooms* receive the materials in the sample\nset. The targets in a room are the walls and flooring. Rooms may be targeted \nby room type or room name. \n\nThe sample set determines *what materials* to randomly select for the\ntarget rooms' walls and floors. \n\nThe sample set is optional and when not specified (null) materials are\nrandomly selected according to the room type for each room in the target\nset.\n\nA sample set with an empty material set is invalid since all wall \nand flooring targets require materials.",
      "type": "object",
      "properties": {
        "Name": {
          "title": "Name",
          "type": "string"
        },
        "TargetSet": {
          "title": "Targetset",
          "anyOf": [
            {
              "$ref": "#/definitions/RoomNameList"
            },
            {
              "$ref": "#/definitions/RoomTypeList"
            }
          ]
        },
        "SampleSet": {
          "$ref": "#/definitions/MaterialSetByMaterialType"
        }
      },
      "required": [
        "Name",
        "TargetSet"
      ],
      "additionalProperties": false
    },
    "InteriorFlooring": {
      "title": "InteriorFlooring",
      "description": "Describes the interior template parameters for all floors for this floorplan.\nAll floors not explicitly targeted will have a random floor material assigned by room type.",
      "type": "object",
      "properties": {
        "MaterialSets": {
          "title": "Materialsets",
          "default": [],
          "type": "array",
          "items": {
            "$ref": "#/definitions/InteriorMaterialSet"
          },
          "minItems": 0,
          "maxItems": 6
        }
      },
      "additionalProperties": false
    },
    "InteriorWalls": {
      "title": "InteriorWalls",
      "description": "Describes the interior template parameters for all walls for this floorplan.\nAll walls not explicitly targeted will have a random wall material assigned by room type.",
      "type": "object",
      "properties": {
        "MaterialSets": {
          "title": "Materialsets",
          "default": [],
          "type": "array",
          "items": {
            "$ref": "#/definitions/InteriorMaterialSet"
          },
          "minItems": 0,
          "maxItems": 6
        }
      },
      "additionalProperties": false
    },
    "ModelTypeList": {
      "title": "ModelTypeList",
      "description": "The set of all models matching any of the listed model types.\nAn empty set means zero models to sample/select.",
      "type": "object",
      "properties": {
        "ModelTypes": {
          "title": "Modeltypes",
          "type": "array",
          "items": {
            "type": "string"
          },
          "minItems": 0
        }
      },
      "required": [
        "ModelTypes"
      ],
      "additionalProperties": false
    },
    "FurnitureArrangementSet": {
      "title": "FurnitureArrangementSet",
      "description": "Describes the interior template for placing furniture in one or more rooms.\n\n- TargetSet is the set of rooms to furnish, filter by room name or room\n  type.\n- SampleSet is a set of all furnishing models to randomly choose and\n  place. \n- DesiredSpatialDensity is the desired level of free space after placing\n  furniture.",
      "type": "object",
      "properties": {
        "Name": {
          "title": "Name",
          "type": "string"
        },
        "TargetSet": {
          "title": "Targetset",
          "anyOf": [
            {
              "$ref": "#/definitions/RoomNameList"
            },
            {
              "$ref": "#/definitions/RoomTypeList"
            }
          ]
        },
        "SampleSet": {
          "$ref": "#/definitions/ModelTypeList"
        },
        "DesiredSpatialDensity": {
          "title": "Desiredspatialdensity",
          "default": "Moderate",
          "enum": [
            "Sparse",
            "Moderate",
            "Dense"
          ],
          "type": "string"
        }
      },
      "required": [
        "Name",
        "TargetSet"
      ],
      "additionalProperties": false
    },
    "InteriorFurnishings": {
      "title": "InteriorFurnishings",
      "description": "Describes the types of furniture models for randomly placing into each room\nin the world.  Rooms are targeted by room type or room name. Rooms that are\nnot targeted are furnished at random by their room type with moderate density.\ndensity. For an empty room, specify an empty sample set.",
      "type": "object",
      "properties": {
        "FurnitureArrangements": {
          "title": "Furniturearrangements",
          "default": [],
          "type": "array",
          "items": {
            "$ref": "#/definitions/FurnitureArrangementSet"
          },
          "minItems": 0,
          "maxItems": 6
        }
      },
      "additionalProperties": false
    },
    "InteriorTemplate": {
      "title": "InteriorTemplate",
      "description": "Top-level template for parameterizing the interior finishes and furnishings for\nthis floorplan.",
      "type": "object",
      "properties": {
        "Flooring": {
          "title": "Flooring",
          "default": {
            "MaterialSets": []
          },
          "allOf": [
            {
              "$ref": "#/definitions/InteriorFlooring"
            }
          ]
        },
        "Walls": {
          "title": "Walls",
          "default": {
            "MaterialSets": []
          },
          "allOf": [
            {
              "$ref": "#/definitions/InteriorWalls"
            }
          ]
        },
        "Furniture": {
          "title": "Furniture",
          "default": {
            "FurnitureArrangements": []
          },
          "allOf": [
            {
              "$ref": "#/definitions/InteriorFurnishings"
            }
          ]
        }
      },
      "additionalProperties": false
    },
    "FloorTemplate": {
      "title": "FloorTemplate",
      "description": "Describes a single foor within a building. Defaults to a single residential room\nof a randomy type and size, and the interior is randomly furnished.",
      "type": "object",
      "properties": {
        "Floorplan": {
          "title": "Floorplan",
          "default": {
            "Footprint": {
              "DesiredAspectRatio": {
                "x": 1.0,
                "y": 1.0
              }
            },
            "Ceiling": {
              "Height": 3.0
            },
            "Rooms": [
              {
                "Type": "Living",
                "Name": "My Living Room",
                "DesiredShape": {
                  "Area": 20.0,
                  "AspectRatio": {
                    "x": 1.0,
                    "y": 1.0
                  }
                }
              },
              {
                "Type": "Bedroom",
                "Name": "My Bedroom",
                "DesiredShape": {
                  "Area": 20.0,
                  "AspectRatio": {
                    "x": 1.0,
                    "y": 1.0
                  }
                }
              }
            ],
            "DesiredConnections": []
          },
          "allOf": [
            {
              "$ref": "#/definitions/FloorplanTemplate"
            }
          ]
        },
        "Interior": {
          "title": "Interior",
          "default": {
            "Flooring": {
              "MaterialSets": []
            },
            "Walls": {
              "MaterialSets": []
            },
            "Furniture": {
              "FurnitureArrangements": []
            }
          },
          "allOf": [
            {
              "$ref": "#/definitions/InteriorTemplate"
            }
          ]
        }
      },
      "additionalProperties": false
    },
    "BuildingTemplate": {
      "title": "BuildingTemplate",
      "description": "Describes a building to be randomly generated. Defaults to one residential floor.",
      "type": "object",
      "properties": {
        "Floors": {
          "title": "Floors",
          "default": [
            {
              "Floorplan": {
                "Footprint": {
                  "DesiredAspectRatio": {
                    "x": 1.0,
                    "y": 1.0
                  }
                },
                "Ceiling": {
                  "Height": 3.0
                },
                "Rooms": [
                  {
                    "Type": "Living",
                    "Name": "My Living Room",
                    "DesiredShape": {
                      "Area": 20.0,
                      "AspectRatio": {
                        "x": 1.0,
                        "y": 1.0
                      }
                    }
                  },
                  {
                    "Type": "Bedroom",
                    "Name": "My Bedroom",
                    "DesiredShape": {
                      "Area": 20.0,
                      "AspectRatio": {
                        "x": 1.0,
                        "y": 1.0
                      }
                    }
                  }
                ],
                "DesiredConnections": []
              },
              "Interior": {
                "Flooring": {
                  "MaterialSets": []
                },
                "Walls": {
                  "MaterialSets": []
                },
                "Furniture": {
                  "FurnitureArrangements": []
                }
              }
            }
          ],
          "type": "array",
          "items": {
            "$ref": "#/definitions/FloorTemplate"
          },
          "minItems": 1,
          "maxItems": 1
        }
      },
      "additionalProperties": false
    }
  }
}
```