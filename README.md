
# Hexo Supabase Comments
Link: https://taijinpei.info/2025/12/15/Supabase-x-Hexo/

A lightweight, real-time comment system for Hexo themes powered by [Supabase](https://supabase.com/). 

Features:
- 💬 **Nested Replies:** Supports deep threading for conversations.
- 🕵️ **Anonymous Mode:** Toggle privacy on or off.
- 🎨 **Customizable Theme:** Clean, modern UI with easy CSS overrides.
- 😃 **Emoji Support:** Built-in emoji picker.
- ⚡ **Fast & Free:** Runs on Supabase's generous free tier.

---

## 🛠️ Prerequisites

1. A **Supabase** account and project.
2. A **Hexo** website.

---

## 1. Supabase Setup

Go to your Supabase project **SQL Editor** and run the following code to create the required table and security policies:

```sql
-- 1. Create the comments table
create table public.comments (
  id uuid not null default gen_random_uuid (),
  created_at timestamp with time zone not null default now(),
  slug text not null,
  name text not null,
  email text null,
  message text not null,
  parent_id uuid null,
  constraint comments_pkey primary key (id),
  constraint comments_parent_id_fkey foreign key (parent_id) references comments (id)
);

-- 2. Enable Row Level Security (RLS)
alter table public.comments enable row level security;

-- 3. Create Policies (Allow anyone to read and insert)
create policy "Enable read access for all users"
on public.comments for select
using (true);

create policy "Enable insert for all users"
on public.comments for insert
with check (true);
````

-----

## 2\. Installation

### Step 1: Add the file

Download `supabase-comments.ejs` and place it in your theme's partials directory:
`themes/your-theme/layout/_partial/supabase-comments.ejs`

### Step 2: Update Configuration

Open your **Theme's** `_config.yml` (e.g., `themes/your-theme/_config.yml`) and add your Supabase credentials:

```yaml
# Supabase Comments System
supabase_comments:
  enable: true
  url: "[https://your-project-ref.supabase.co](https://your-project-ref.supabase.co)"
  key: "your-anon-public-key"
```

*\> **Note:** If you prefer to use the root site `_config.yml`, ensure you update the variable in the `.ejs` file from `theme.supabase_comments` to `config.supabase_comments`.*

### Step 3: Inject into Post Layout

Open your post layout file (usually `themes/your-theme/layout/post.ejs` or `article.ejs`) and add this where you want the comments to appear:

```ejs
<% if (theme.supabase_comments.enable) { %>
  <%- partial('_partial/supabase-comments') %>
<% } %>
```

-----

## 🎨 Customization

The file contains a `<style>` block at the bottom. You can easily change the color scheme by modifying the CSS variables at the top of that block:

```css
:root {
  --supa-theme-gradient: linear-gradient(to right, #4cbf30 0%, #0f9d58 100%);
  --supa-theme-solid: #0f9d58; 
}
```
