# dependency-graph

Python script for auto-generating product dependency graphs using Mermaid.

## Usage

1. Export product Excel sheet to CSV
2. Generate mermaid code
   ```
   python mermaid.py myfile.csv
   ```
3. Copy and paste output into the [Mermaid Online Editor ](https://mermaid.live)

## Examples

To view the Mermaid code, click [here](https://github.com/cmsaf/dependency-graph/blob/main/README.md?plain=1).


### Dependency Graph (auto-generated)

Here's a diagram generated from the `clara.csv` file in this repo. Note that there are no
connections to usage examples. This is not supported by the simple data model in the
Excel sheet, so we'll add them manually. We will also tweak the layout a bit by allowing
Mermaid to make certain arrows longer (by default it tries to minimize arrow length).

```mermaid
flowchart TD
    subgraph Clouds["Clouds"]
        CFC_CDR[("CM-11014<br/>Fractional Cloud Cover")]
        CFC_ICDR["CM-6013<br/>Fractional Cloud Cover"]
    end

    subgraph Albedo["Albedo"]
        BAL_CDR[("CM-12231<br/>Blue-sky Albedo")]
        BAL_ICDR["CM-6231<br/>Blue-sky Albedo"]
    end

    subgraph ToA_Radiation["ToA Radiation"]
        RSF_CDR[("CM-11314<br/>TOA Reflected Solar Flux")]
        RSF_ICDR["CM-6333<br/>TOA Reflected Solar Flux"]
    end

    subgraph Surface_Radiation["Surface Radiation"]
        SDL_CDR[("CM-11264<br/>Surface Downward Longwave Radiation")]
        SNL_CDR[("CM-11293<br/>Surface Net Longwave Radiation")]
        SRB_CDR[("CM-11274<br/>Surface Radiation Budget")]
        SIS_CDR[("CM-11204<br/>Surface Incoming Shortwave Radiation")]
        SNS_CDR[("CM-11283<br/>Surface Net Shortwave Radiation")]
        SDL_ICDR["CM-6263<br/>Surface Downward Longwave Radiation"]
        SNL_ICDR["CM-6293<br/>Surface Net Longwave Radiation"]
        SRB_ICDR["CM-6273<br/>Surface Radiation Budget"]
        SIS_ICDR["CM-6213<br/>Surface Incoming Shortwave Radiation"]
        SNS_ICDR["CM-6283<br/>Surface Net Shortwave Radiation"]
    end

    subgraph Anomalies["Anomalies"]
        SIS_NORMAL[("CM-81204<br/>Normal Surface Incoming Shortwave Radiation")]
        SIS_ANOMALY["CM-8213<br/>Anomaly of Surface Incoming Shortwave Radiation"]
    end

    subgraph Usage_examples["Usage examples"]
        DWD_ICDR["Climate Monitoring over Europe on behalf of WMO<br/>(German Meteorological Service)"]
        SMHI_ICDR["Legal obligation for national climate monitoring<br/>(Swedish Meteorological and Hydrological Institute)"]
        GER_ICDR["German climate attribution portal<br/>(German Government)"]
    end

    CFC_CDR --> SDL_CDR
    SDL_CDR --> SNL_CDR
    SNL_CDR --> SRB_CDR
    SNS_CDR --> SRB_CDR
    CFC_CDR --> SIS_CDR
    RSF_CDR --> SIS_CDR
    SIS_CDR --> SNS_CDR
    BAL_CDR --> SNS_CDR
    CFC_ICDR --> SDL_ICDR
    SDL_ICDR --> SNL_ICDR
    SNL_ICDR --> SRB_ICDR
    SNS_ICDR --> SRB_ICDR
    CFC_ICDR --> SIS_ICDR
    RSF_ICDR --> SIS_ICDR
    SIS_ICDR --> SNS_ICDR
    BAL_ICDR --> SNS_ICDR
    SIS_CDR --> SIS_NORMAL
    SIS_ICDR --> SIS_ANOMALY
    SIS_NORMAL --> SIS_ANOMALY

    classDef cm fill:#fece79, color:#000
    classDef osi fill:#67d0f7, color:#000
    classDef eum fill:#507891, color:#000
    classDef ext fill:#bbbbbb, color:#000
    classDef sub fill:#f5f5f5

    class CFC_CDR cm
    class BAL_CDR cm
    class RSF_CDR cm
    class SDL_CDR cm
    class SNL_CDR cm
    class SRB_CDR cm
    class SIS_CDR cm
    class SNS_CDR cm
    class CFC_ICDR cm
    class BAL_ICDR cm
    class RSF_ICDR cm
    class SDL_ICDR cm
    class SNL_ICDR cm
    class SRB_ICDR cm
    class SIS_ICDR cm
    class SNS_ICDR cm
    class SIS_NORMAL cm
    class SIS_ANOMALY cm
    class DWD_ICDR ext
    class SMHI_ICDR ext
    class GER_ICDR ext
    class Clouds sub
    class Albedo sub
    class ToA_Radiation sub
    class Surface_Radiation sub
    class Anomalies sub
    class Usage_examples sub
```

### Overview (created manually)

```mermaid
--- 
title: CLARA-A Datasets
---
graph LR


era5["ERA-5(T)"<br>ECMWF]:::ext
fdr["F(C)DR<br>AVHRR/VIIRS"]:::eum
ice_conc_tcdr[(OSI-111<br>Ice conc.)]:::osi
ice_conc_icdr[OSI-111<br>Ice conc.]:::osi

subgraph clara4[CLARA-A4]
    direction LR
    clara4_tcdr[(CLARA-A4 CDR<br><span style='font-size: 12px'>-Clouds<br>-Surf.Radiation<br>-Albedo<br>-ToA Radiation)]:::cm
    clara4_icdr[CLARA-A4 ICDR]:::cm
    clara4_normal[(CLARA-A4 Normal)]:::cm
    clara4_anom[CLARA-A4 Anomalies]:::cm
end

clara35["CLARA-A3.5 (I)CDR & Anomalies"]:::cm

subgraph applications[Application Areas]
    direction LR
    clim_mon[Climate Monitoring]:::ext
    clim_change[Climate Change Analysis]:::ext
    clim_impact[Climate Impact Analysis]:::ext
    clim_model[Climate Modelling and Evaluation]:::ext
    energy[Renewable Energy]:::ext
    public[Public Sector and Government Agencies]:::ext
end

era5 -- input to --> clara4
fdr -- input to --> clara4
ice_conc_tcdr -- input to --> clara4
ice_conc_icdr -- input to --> clara4

clara4_tcdr -- extended by --> clara4_icdr
clara4_tcdr -- input to --> clara4_normal
clara4_icdr -- input to --> clara4_anom
clara4_normal -- input to --> clara4_anom

clara4 -- input to --> applications

clara35 -. superseded by .-> clara4

%% class definitions

class clara4,applications sub
class clara4_tcdr,clara4_icdr,clara4_normal,clara4_anom new

classDef cm fill:#fece79, color:#000
classDef osi fill:#67d0f7, color:#000
classDef eum fill:#507891, color:#000
classDef ext fill:#bbbbbb, color:#000
classDef sub fill:#f5f5f5
classDef new stroke-width:3px, font-weight:bold
```

```mermaid

graph LR

subgraph legend[Legend]
    eum_legend[EUMETSAT]:::eum
    cm_legend[CM SAF]:::cm
    osi_legend[OSI SAF]:::osi
    ext_legend[External]:::ext
end

%% class definitions

class legend sub

classDef cm fill:#fece79, color:#000
classDef osi fill:#67d0f7, color:#000
classDef eum fill:#507891, color:#000
classDef ext fill:#bbbbbb, color:#000
classDef sub fill:#f5f5f5
classDef new stroke-width:3px, font-weight:bold
```

## General Diagram Guidelines

For reference, these are the general diagram guidelines.

* Products are represented by elements ("nodes") identified by their Product Identifier
* The first text element is the Product identifier as the only mandatory element,
  followed by the product name in the second row. Further specification can be provided
  in the following rows as necessary.

### Product categories

Different product types are characterised through their shape

```mermaid
graph TD 
data[dynamic<br>dataset]
datarecord[(static <br> dataset)]

software[[software<br>product]]

other{{service<br>product}}
```

Newly proposed product commitments should use bold text and thicker box outlines

```mermaid
graph TD 
Existing[TST-123 <br> Test V2.1 <br>operational]
New[TST-123 <br> Test V2.2] 

class New new
classDef new font-weight:bold, stroke-width:3px;
class Existing lsa
class New lsa
classDef lsa fill:#f79b6a, color:#000
```

### Connectors

Relation and dependencies between products can be assigned via connectors and arrows.

- **Solid arrow**: "input to"
- **Dashed arrow**: "superseded by"
- **Dashed arrow**: "to be discontinued"

```mermaid
flowchart LR

A -- input to --> B

old[Old Product] -. superseded by .-> new[New Product]

olda[Old Product] -. to be discontinued .-> X((X))

class new rom
class old rom
class olda ac
class new new
class X stop

classDef new stroke-width:3px, font-weight:bold
classDef rom fill:#a3c179, color:#000
classDef ac fill:#ae8cbf, color:#000
classDef stop fill:#ff0000
```
