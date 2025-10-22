# Salon Owner Lead Capture Wizard

A mobile-first 4-step wizard form designed to capture qualified salon owner leads from Instagram/Facebook ads.

## Features

- ✨ **4-Step Wizard Flow**: About You → Salon Details → Media → Review
- 📱 **Mobile-Optimized**: Touch-friendly UI with sticky navigation
- ⚠️ **OTP Verification**: Temporarily disabled (no SMS service needed for deployment)
- 📍 **Location Detection**: Auto-fill city/pincode using GPS (Nominatim reverse geocoding)
- 📸 **Image Uploads**: Menu card + gallery photos with client-side compression
- 💾 **Auto-Save**: Form data persists in localStorage
- 🚀 **Webhook Integration**: Submits data to external endpoint
- 📊 **UTM Tracking**: Captures campaign source parameters

## Tech Stack

- **Frontend**: React + TypeScript + Tailwind CSS
- **Backend**: Express.js + Node.js
- **Image Processing**: browser-image-compression (1MB max, 1600px)
- **Storage**: In-memory (can be upgraded to database)

## Setup

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Configure environment variables**:
   Create a `.env` file in the root directory:
   ```env
   WEBHOOK_URL=https://your-webhook-endpoint.com/lead
   ```

3. **Start the development server**:
   ```bash
   npm run dev
   ```

4. **Access the application**:
   Open http://localhost:5000 in your browser

## How It Works

### Step 1: About You
- Full name input
- Mobile number with +91 prefix (10 digits)
- ⚠️ **OTP verification temporarily disabled** (for quick deployment)
- Duplicate submission prevention (10-minute cooldown)

### Step 2: Salon Details
- Salon/outlet name
- Google Maps link (validated)
- **Location Detection**: One-click "Use My Location" button to auto-fill city and pincode using GPS
- City and pincode (manual entry or auto-filled)
- Optional full address

### Step 3: Media Upload
- **Menu Card**: Single image upload (required)
- **Gallery**: 3-8 photos (recommended)
- Camera capture support on mobile
- Automatic compression to ≤1MB

### Step 4: Review & Submit
- Summary of all entered information
- Edit links to previous steps
- Consent checkbox with privacy policy link
- Final submission to webhook

## API Endpoints

### POST `/api/otp/send`
Send OTP to mobile number (mock implementation)
```json
{
  "mobile": "9876543210"
}
```

### POST `/api/otp/verify`
Verify OTP code
```json
{
  "mobile": "9876543210",
  "otp": "123456"
}
```

### POST `/api/upload`
Upload image file
- Accepts: `multipart/form-data`
- Returns: `{ "success": true, "url": "/uploads/filename.jpg" }`

### POST `/api/leads`
Submit complete lead form
```json
{
  "fullName": "John Doe",
  "mobile": "+919876543210",
  "otpVerified": true,
  "salonName": "Beauty Salon",
  "googleMapsUrl": "https://maps.app.goo.gl/...",
  "city": "Mumbai",
  "pincode": "400001",
  "address": "123 Main St",
  "menuCardUrl": "/uploads/menu.jpg",
  "galleryUrls": ["/uploads/1.jpg", "/uploads/2.jpg"],
  "source": {
    "utm_source": "facebook",
    "utm_medium": "cpc",
    "utm_campaign": "salon-leads"
  },
  "userAgent": "...",
  "consent": true
}
```

## Webhook Payload

When a lead is submitted, the following JSON is POSTed to your webhook URL:

```json
{
  "full_name": "John Doe",
  "mobile": "+919876543210",
  "otp_verified": true,
  "salon_name": "Beauty Salon",
  "google_maps_url": "https://maps.app.goo.gl/...",
  "city": "Mumbai",
  "pincode": "400001",
  "address": "123 Main St",
  "menu_card_url": "/uploads/menu.jpg",
  "gallery_urls": ["/uploads/1.jpg", "/uploads/2.jpg"],
  "source": {
    "utm_source": "facebook",
    "utm_medium": "cpc",
    "utm_campaign": "salon-leads",
    "fbclid": "...",
    "referrer": "https://facebook.com"
  },
  "client_ts": "2025-10-16T12:34:56.789Z",
  "user_agent": "Mozilla/5.0...",
  "consent": true
}
```

## OTP Implementation (Currently Disabled)

⚠️ **OTP verification is temporarily disabled** to allow deployment without SMS service integration.

The app currently:
- Collects mobile numbers without verification
- Sets `otpVerified: false` in all submissions
- Displays a notice to users about disabled verification

**To enable OTP in production**, integrate with:
- **MSG91** (~₹0.15-0.25 per SMS) - Best for India
- **Fast2SMS** (~₹0.10-0.15 per SMS) - Cheapest option
- **Twilio** (~₹0.50-1.00 per SMS) - Global standard
- **AWS SNS** (~₹0.50 per SMS) - AWS ecosystem

**Re-enable OTP**: Uncomment OTP UI in `WizardForm.tsx` and integrate SMS service

## Image Storage

Images are currently stored in the `/uploads` directory on the server. For production:

1. **Cloud Storage** (Recommended):
   - AWS S3
   - Cloudinary
   - Firebase Storage
   - Google Cloud Storage

2. **Update the upload endpoint** to return full URLs:
   ```javascript
   const url = `https://your-cdn.com/uploads/${filename}`;
   ```

## Customization

### Colors
Edit `client/src/index.css` to customize the primary purple color:
```css
--primary: 280 65% 60%; /* HSL values */
```

### Validation
Modify validation rules in `client/src/pages/WizardForm.tsx`:
- Phone number format
- Google Maps URL patterns
- Image count limits
- Form field requirements

### Success Screen
Customize the success message and action buttons in `client/src/components/SuccessScreen.tsx`

## UTM Parameter Tracking

The form automatically captures UTM parameters from the URL:
- `utm_source` - Traffic source (e.g., facebook, instagram)
- `utm_medium` - Marketing medium (e.g., cpc, social)
- `utm_campaign` - Campaign name
- `fbclid` - Facebook click identifier
- `referrer` - HTTP referrer

Example URL:
```
https://yourapp.com/?utm_source=facebook&utm_medium=cpc&utm_campaign=salon-owners-2025
```

## Deployment

### Vercel/Netlify
1. Set environment variable: `WEBHOOK_URL`
2. Deploy the project
3. Ensure the `/uploads` directory is writable (or switch to cloud storage)

### Custom Server
1. Build the project: `npm run build`
2. Set environment variables
3. Start the server: `npm start`

## License

MIT
