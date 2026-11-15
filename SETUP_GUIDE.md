# Wholesale Product Catalog Website — Setup Guide

This is a ready-to-configure website starter. It includes:
- Public product catalog with search and categories
- Admin sign-in, add/edit/delete products, image upload
- Order form and admin order management/status updates
- Private products hidden from public browsing
- One-time private links (first successful opening consumes the link on the server)

## Important: free hosting and backend
Use **Supabase Free** for database, authentication and image storage, and **GitHub Pages** for free static hosting. Free-plan limits and availability can change. You need to create your own accounts and project; this package does not contain hosted credentials or a live domain.

## Setup

### 1. Create Supabase project
1. Go to https://supabase.com and create a free project.
2. Open **SQL Editor**.
3. Open `supabase/schema.sql`, copy all SQL and run it.
4. In Supabase **Authentication > Users**, create your admin user with your own email and a strong password.
5. Copy that user's UUID.
6. In SQL Editor, run:
   `insert into public.admin_users(user_id) values ('PASTE-ADMIN-USER-UUID-HERE');`
7. Open **Project Settings > API** and copy the Project URL and anon/public key.

### 2. Set your shop details and connect the website
1. In `index.html`, set `WHATSAPP_NUMBER` to your Bangladesh WhatsApp number in international format without `+` or spaces (example format: `88017XXXXXXXX`). This enables the WhatsApp order button.
2. The shop name is already set to **Wholesale Bazar**.
3. Products with a price entered show that price; leave the price blank to display **Price on request**. Both website-form orders and WhatsApp orders are available on product cards.

### 3. Connect the website
Open `index.html` in a text editor. Replace:
- `YOUR_SUPABASE_PROJECT_URL`
- `YOUR_SUPABASE_ANON_KEY`

with your Supabase Project URL and anon/public key. Never put a `service_role` key in the website.

### 4. Test before publishing
1. Open `index.html` in a browser through a local static server or publish it.
2. Press Admin and sign in.
3. Add a test public product and upload a photo.
4. Open the public catalog in a private/incognito browser window and confirm the product appears.
5. Add a private product. It should not appear in the public catalog.
6. For a private link to work, the product must be marked Private. In Admin > One-time link, generate and copy a link. The first successful opening redeems it. Reloading or opening it again should show an invalid/already-used message.
7. Place a test order and check Admin > Orders.

### 5. Publish free with GitHub Pages
1. Create a GitHub account and a new public repository.
2. Upload `index.html` to the repository root.
3. Open repository **Settings > Pages**.
4. Choose deploy from branch `main`, folder `/root`, and Save.
5. GitHub will show your public website URL after deployment.

## How the private one-time link works
The website generates a random token. Only its SHA-256 hash is stored in the database. When the link is opened, a database function atomically marks it as redeemed and returns the private product only to the first successful request. If the recipient forwards the link before opening it, whoever opens it first gets access. This confirms one-time use, not the identity of the recipient. To restrict access to a named customer, add phone/email OTP verification.

## Notes / production hardening
- The order form records customer name, phone, address, items and notes. Handle this personal information carefully.
- Before public launch, add your shop name, logo, contact number, delivery policy, return policy and terms.
- Product prices are optional and can be shown as "Price on request".
- Do not publish prescription/medical claims as guaranteed outcomes; keep product descriptions factual and consistent with approved packaging.
