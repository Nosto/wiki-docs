# Using analytics and A/B testing with search

```mermaid
flowchart TD
    Session[Create Session] -.-> Segments[Fetch segments]
    Segments --> Search
    Search --> Impression[Track search impression]
    Search --> ABTO["Store A/B variations\n(if applicable)"]
    Impression --> Display[Display results]
    ABTO --> Display

    Display -.->|on result click| Click[Track search click]
    Click -.-> Segments
    Display -.-> Segments
```
