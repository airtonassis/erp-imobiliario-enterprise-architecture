# Ele mostrará como os documentos se relacionam

| Documento             | Depende de              | Alimenta              |
| --------------------- | ----------------------- | --------------------- |
| README                | —                       | Todos                 |
| PROJECT_OVERVIEW      | README                  | Charter               |
| PROJECT_CHARTER       | Overview                | Business Architecture |
| BUSINESS_ARCHITECTURE | Charter                 | Todos os Domínios     |
| FRAMEWORK_GUIDE       | Architecture Principles | Backend               |
| PROPERTY README       | Business Architecture   | Entities              |
| ENTITIES              | README                  | Use Cases             |
| USE_CASES             | ENTITIES                | Código                |
