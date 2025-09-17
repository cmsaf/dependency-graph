# dependency-graph

Python script for auto-generating product dependency graphs using Mermaid.

## Usage

1. Export product Excel sheet to CSV
2. Generate mermaid code
   ```
   python mermaid.py myfile.csv
   ```
3. Copy and paste output into the [Mermaid Online Editor ](https://mermaid.live)

## Example

Here's a diagram generated from the `clara.csv` file in this repo.

Note that there are no connections to usage examples. This is not supported by the
simple data model in the Excel sheet. We'll add them manually.

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
