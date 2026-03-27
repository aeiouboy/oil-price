# Oil Price Dashboard

Oil price monitoring dashboard powered by Bangchak API. Displays real-time fuel prices, historical comparison, and distance calculator by car type.

## Features

- Real-time oil price cards (all fuel types)
- Today vs Yesterday comparison widget
- Bar chart comparison across fuel types
- Distance calculator by car type (Eco Car / Sedan / Pickup)
- Auto-refresh every 60 seconds
- Responsive design (mobile / tablet / desktop)

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | Next.js 16 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS v4 |
| UI Components | shadcn/ui |
| Charts | Recharts |
| Data Fetching | SWR |
| Icons | Lucide React |

## Data Source

```
GET https://oil-price.bangchak.co.th/ApiOilPrice2/th
```

## Documentation

- [PRD](docs/PRD-oil-price-dashboard.md) - Product Requirements Document
