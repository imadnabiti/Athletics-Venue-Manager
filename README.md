# Athletics Venue Manager

A specialized platform engineered to catalog, map, inspect, and evaluate track and field athletic facilities. The application enables technical tracking of running tracks, runway dispositions, equipment inventory, and facility defects, accompanied by an administrative bulletin system and strict role-based access control.

## 📌 Key Features

* **Facility Cataloging & Condition Tracking**: Centrally record facility locations, lane configurations, surface wear, spatial dimensions, and critical operational defects.
* **Interactive Runway & Equipment Layout Editor**: Position, rotate, and zoom runway apparatus, jumping pits, and landing mats directly on vector/raster venue planimetries.
* **Technical Factsheet Export**: Instant single-click PDF generation summarizing facility parameters, current inspection notes, and runway layouts.
* **Administrative Bulletin Board**: Publish announcements, system updates, and inspection alerts directly to user dashboards.
* **Role-Based Access Control (RBAC)**:
  * **Admin**: Full authority to create/edit facility records, adjust runway planimetries, log maintenance issues, and manage notices.
  * **Viewer / Visitor**: Read-only access to search facilities, review visual equipment maps, and download technical PDF dossiers.

---

## 🗄️ Database Architecture

The data layer is structured around two core relational tables:

### 1. `athletics_facilities`
Stores facility profiles, geographic coordinates, track conditions, media links, and technical runway JSON layouts.

| Column Name | Type / Format | Description |
| :--- | :--- | :--- |
| `id` | Identifier | Unique record ID |
| `name` | String | Facility name |
| `municipality` | String | Municipality / City |
| `address` | String | Street address |
| `latitude` | Decimal | Geographic latitude coordinate |
| `longitude` | Decimal | Geographic longitude coordinate |
| `lanes` | String | Lane configuration (e.g., number of lanes, indoor/outdoor) |
| `maintenance_status` | String | Overall surface and maintenance condition |
| `photo_url` | String (URL) | Link to facility image in cloud storage |
| `planimetry_url` | String (URL) | Link to track map or layout planimetry |
| `equipment` | Text / String | Available sports equipment summary |
| `issues` | Text / String | Reported facility issues, damage, or safety notes |
| `dimension_notes` | Text / String | Straightaway lengths, curve radius, and spatial notes |
| `runways_list` | JSON | Coordinates and technical data (`layout_visivo`, `dati_tecnici`) for runway equipment |

### 2. `bulletin_notices`
Powers the announcement dashboard for news, version releases, and operational alerts.

| Column Name | Type / Format | Description |
| :--- | :--- | :--- |
| `id` | UUID | Unique announcement identifier |
| `title` | String | Notice headline |
| `description` | Text | Detailed notice body |
| `tag_data` | String | Metadata badge or version tag (e.g., version number, category) |
| `icon` | String (Emoji/Text) | Display icon for the alert card |
| `display_order` | Integer | Sorting sequence index |
| `is_active` | Boolean | Visibility toggle flag |
| `created_at` | Timestamp | Creation timestamp |

---

## 👥 User Roles & Permissions

| Role | Permissions | Scope |
| :--- | :--- | :--- |
| **Admin** | Full read, write, update, delete | Manage facilities, edit layouts, publish bulletin notices, configure roles |
| **Visitor** | Read-only | Consult facility cards, inspect equipment layouts, export technical PDF factsheets |

---

## 🚀 Getting Started

### Prerequisites

* A [Supabase](https://supabase.com/) project
* A [Vercel](https://vercel.com/) account
* [Node.js](https://nodejs.org/) (v18 or newer)
* Git

### 1. Supabase Setup

1. Create a new project in the **Supabase Dashboard**.
2. Open the **SQL Editor** in your Supabase project and execute the schema creation queries for `athletics_facilities` and `bulletin_notices`.
3. Set up a public storage bucket (e.g., `facility-media`) for facility photographs and track planimetries.
4. Go to **Project Settings > API** and copy your **Project URL** and **`anon` Public Key** (plus the `service_role` key if performing administrative backend tasks).

### 2. Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/athletics-venue-manager.git
   cd athletics-venue-manager
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Configure environment variables:**
   Create a `.env.local` file in the root directory:
   ```env
   NEXT_PUBLIC_SUPABASE_URL=https://your-project-id.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
   SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
   ```

4. **Run the local development server:**
   ```bash
   npm run dev
   ```
   Open `http://localhost:3000` in your browser.

### 3. Deploying to Vercel

#### Option A: Via Vercel Dashboard (Recommended)

1. Push your code to a GitHub repository.
2. Log into the [Vercel Dashboard](https://vercel.com/) and click **Add New > Project**.
3. Import your `athletics-venue-manager` repository.
4. In the **Environment Variables** section, add the same keys configured locally:
   * `NEXT_PUBLIC_SUPABASE_URL`
   * `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   * `SUPABASE_SERVICE_ROLE_KEY`
5. Click **Deploy**.

#### Option B: Via Vercel CLI

```bash
npm install -g vercel
vercel login
vercel link
vercel env pull .env.local
vercel --prod
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
