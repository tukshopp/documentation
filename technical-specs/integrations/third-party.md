# Third-Party Integrations

**Version:** 1.0  
**Last Updated:** November 11, 2025

---

## 📋 Overview

TukShopp integrates with several third-party services to provide comprehensive functionality including image hosting, email delivery, push notifications, and messaging.

### Integrated Services
1. **Cloudinary** - Media management and CDN
2. **ZeptoMail** - Transactional email service
3. **Firebase** - Push notifications and real-time features
4. **WhatsApp Business API (Facebook)** - Messaging and notifications

---

## 🖼️ Cloudinary Integration

### Overview
**Purpose:** Image and media asset management  
**Website:** [https://cloudinary.com/](https://cloudinary.com/)  
**Plan:** [Specify plan]

### Use Cases
- Menu item images
- Vendor logos and banners
- User profile pictures
- Delivery proof photos
- Document scans

### Configuration

#### Credentials
```env
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

#### SDK Setup (Node.js)
```javascript
const cloudinary = require('cloudinary').v2;

cloudinary.config({ 
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME, 
  api_key: process.env.CLOUDINARY_API_KEY, 
  api_secret: process.env.CLOUDINARY_API_SECRET,
  secure: true
});
```

### Implementation

#### Upload Image
```javascript
async function uploadImage(filePath, folder, publicId) {
  try {
    const result = await cloudinary.uploader.upload(filePath, {
      folder: folder,
      public_id: publicId,
      resource_type: 'auto',
      transformation: [
        { width: 1000, height: 1000, crop: 'limit' },
        { quality: 'auto', fetch_format: 'auto' }
      ]
    });
    
    return {
      url: result.secure_url,
      publicId: result.public_id,
      format: result.format,
      width: result.width,
      height: result.height
    };
  } catch (error) {
    console.error('Cloudinary upload error:', error);
    throw error;
  }
}
```

#### Delete Image
```javascript
async function deleteImage(publicId) {
  try {
    const result = await cloudinary.uploader.destroy(publicId);
    return result.result === 'ok';
  } catch (error) {
    console.error('Cloudinary delete error:', error);
    throw error;
  }
}
```

#### Generate Optimized URL
```javascript
function getOptimizedUrl(publicId, options = {}) {
  return cloudinary.url(publicId, {
    secure: true,
    transformation: [
      { width: options.width || 500, crop: 'scale' },
      { quality: 'auto', fetch_format: 'auto' }
    ]
  });
}
```

### Folder Structure
```
/tukshopp/
  /vendors/
    /{vendor_id}/
      /logo
      /banner
      /menu_items
  /users/
    /{user_id}/
      /profile
  /deliveries/
    /{delivery_id}/
      /proof
```

### Image Transformations

#### Responsive Images
```javascript
// Thumbnail
const thumbnail = cloudinary.url(publicId, {
  transformation: [
    { width: 200, height: 200, crop: 'fill', gravity: 'auto' },
    { quality: 'auto', fetch_format: 'auto' }
  ]
});

// Mobile
const mobile = cloudinary.url(publicId, {
  transformation: [
    { width: 800, crop: 'scale' },
    { quality: 'auto', fetch_format: 'auto' }
  ]
});

// Desktop
const desktop = cloudinary.url(publicId, {
  transformation: [
    { width: 1200, crop: 'scale' },
    { quality: 'auto', fetch_format: 'auto' }
  ]
});
```

### Error Handling
- **Upload failures:** Retry with exponential backoff
- **Network errors:** Queue for retry
- **Invalid formats:** Validate before upload
- **Size limits:** Compress before upload

### Monitoring
- Track upload success/failure rates
- Monitor bandwidth usage
- Alert on quota approaching limit
- Monthly usage reviews

### Cost Optimization
- Use auto-quality for compression
- Lazy load images
- Implement caching
- Archive unused images

---

## 📧 ZeptoMail Integration

### Overview
**Purpose:** Transactional email delivery  
**Website:** [https://www.zeptomail.com/](https://www.zeptomail.com/)  
**Plan:** [Specify plan]

### Use Cases
- Welcome emails
- Order confirmations
- Password reset
- OTP verification
- Vendor notifications
- Payout confirmations
- Promotional emails

### Configuration

#### Credentials
```env
ZEPTOMAIL_API_KEY=your_api_key
ZEPTOMAIL_FROM_EMAIL=noreply@tukshopp.ng
ZEPTOMAIL_FROM_NAME=TukShopp
```

#### SDK Setup
```javascript
const { SendMailClient } = require('zeptomail');

const client = new SendMailClient({
  apiKey: process.env.ZEPTOMAIL_API_KEY
});
```

### Implementation

#### Send Transactional Email
```javascript
async function sendEmail({ to, subject, templateKey, variables }) {
  try {
    const result = await client.sendMail({
      from: {
        address: process.env.ZEPTOMAIL_FROM_EMAIL,
        name: process.env.ZEPTOMAIL_FROM_NAME
      },
      to: [
        {
          email_address: {
            address: to.email,
            name: to.name
          }
        }
      ],
      subject: subject,
      htmlbody: renderTemplate(templateKey, variables)
    });
    
    return {
      success: true,
      messageId: result.message_id
    };
  } catch (error) {
    console.error('ZeptoMail error:', error);
    throw error;
  }
}
```

### Email Templates

#### Welcome Email
```javascript
const welcomeEmail = {
  to: { email: user.email, name: user.name },
  subject: 'Welcome to TukShopp! 🎉',
  templateKey: 'welcome',
  variables: {
    userName: user.first_name,
    verificationUrl: verificationUrl,
    loginUrl: 'https://tukshopp.ng/login'
  }
};
```

#### Order Confirmation
```javascript
const orderConfirmation = {
  to: { email: customer.email, name: customer.name },
  subject: `Order Confirmation - ${orderNumber}`,
  templateKey: 'order-confirmation',
  variables: {
    orderNumber: orderNumber,
    orderItems: items,
    totalAmount: total,
    deliveryAddress: address,
    trackingUrl: trackingUrl
  }
};
```

#### Password Reset
```javascript
const passwordReset = {
  to: { email: user.email, name: user.name },
  subject: 'Reset Your Password',
  templateKey: 'password-reset',
  variables: {
    resetUrl: resetUrl,
    expiryTime: '1 hour'
  }
};
```

### Email Queue System
```javascript
// Queue emails for retry
class EmailQueue {
  async queueEmail(emailData) {
    // Add to queue (Redis, MongoDB, etc.)
    await queue.add('send-email', emailData, {
      attempts: 3,
      backoff: {
        type: 'exponential',
        delay: 2000
      }
    });
  }
  
  async processQueue() {
    queue.process('send-email', async (job) => {
      return await sendEmail(job.data);
    });
  }
}
```

### Monitoring
- Track delivery rates
- Monitor bounce rates
- Alert on high failure rates
- Track open rates (if tracking enabled)

### Best Practices
- Validate email addresses before sending
- Use templates for consistency
- Include unsubscribe links (for marketing)
- Comply with email regulations (CAN-SPAM, GDPR)
- Test emails before production use

---

## 🔔 Firebase Integration

### Overview
**Purpose:** Push notifications and real-time features  
**Website:** [https://firebase.google.com/](https://firebase.google.com/)  
**Services Used:**
- Firebase Cloud Messaging (FCM)
- Real-time Database (optional)
- Firebase Analytics (optional)

### Use Cases
- Push notifications to mobile apps
- Real-time location tracking
- Real-time order updates
- In-app messaging
- User analytics

### Configuration

#### Server Configuration
```env
FIREBASE_PROJECT_ID=tukshopp-app
FIREBASE_SERVER_KEY=your_server_key
FIREBASE_SENDER_ID=your_sender_id
```

#### Service Account
```json
{
  "type": "service_account",
  "project_id": "tukshopp-app",
  "private_key_id": "...",
  "private_key": "...",
  "client_email": "firebase-adminsdk@tukshopp-app.iam.gserviceaccount.com",
  "client_id": "...",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token"
}
```

#### SDK Setup (Node.js)
```javascript
const admin = require('firebase-admin');

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: 'https://tukshopp-app.firebaseio.com'
});
```

### Push Notifications (FCM)

#### Send Notification
```javascript
async function sendPushNotification({ userId, title, body, data }) {
  try {
    // Get user's FCM token from database
    const fcmToken = await getUserFCMToken(userId);
    
    const message = {
      notification: {
        title: title,
        body: body
      },
      data: data || {},
      token: fcmToken
    };
    
    const response = await admin.messaging().send(message);
    console.log('Successfully sent message:', response);
    return response;
  } catch (error) {
    console.error('Error sending message:', error);
    throw error;
  }
}
```

#### Notification Types

**Order Status Update:**
```javascript
await sendPushNotification({
  userId: customerId,
  title: 'Order Update',
  body: 'Your order is on the way! 🚴',
  data: {
    type: 'order_update',
    orderId: orderId,
    status: 'on_the_way',
    action: 'track_order'
  }
});
```

**New Order (Vendor):**
```javascript
await sendPushNotification({
  userId: vendorId,
  title: 'New Order! 🔔',
  body: `Order #${orderNumber} - ₦${amount}`,
  data: {
    type: 'new_order',
    orderId: orderId,
    action: 'view_order'
  }
});
```

**Delivery Assignment (Rider):**
```javascript
await sendPushNotification({
  userId: riderId,
  title: 'New Delivery Request',
  body: `Pickup from ${vendorName}`,
  data: {
    type: 'delivery_request',
    deliveryId: deliveryId,
    action: 'view_delivery'
  }
});
```

#### Topic-Based Messaging
```javascript
// Subscribe user to topic
await admin.messaging().subscribeToTopic(fcmToken, 'promotions');

// Send to topic
await admin.messaging().send({
  notification: {
    title: 'Weekend Special! 🎉',
    body: '20% off on all orders today!'
  },
  topic: 'promotions'
});
```

### Real-time Database (If Used)

#### Update Live Location
```javascript
async function updateRiderLocation(riderId, latitude, longitude) {
  const ref = admin.database().ref(`riders/${riderId}/location`);
  await ref.set({
    latitude: latitude,
    longitude: longitude,
    timestamp: admin.database.ServerValue.TIMESTAMP
  });
}
```

#### Listen to Location Updates
```javascript
// Client-side (mobile app)
const locationRef = firebase.database().ref(`riders/${riderId}/location`);
locationRef.on('value', (snapshot) => {
  const location = snapshot.val();
  // Update map with new location
  updateMapMarker(location.latitude, location.longitude);
});
```

### Error Handling
- Handle invalid tokens (user uninstalled app)
- Retry failed notifications
- Track delivery failures
- Clean up expired tokens

### Monitoring
- Track notification delivery rates
- Monitor failed sends
- Alert on high failure rates
- Track user engagement with notifications

---

## 💬 WhatsApp Business API Integration

### Overview
**Purpose:** Customer messaging and notifications  
**Provider:** Facebook (Meta)  
**Website:** [https://developers.facebook.com/docs/whatsapp](https://developers.facebook.com/docs/whatsapp)

### Use Cases
- Order status updates
- Delivery notifications
- Customer support
- OTP verification
- Promotional messages (with opt-in)

### Configuration

#### Credentials
```env
WHATSAPP_PHONE_NUMBER_ID=your_phone_number_id
WHATSAPP_BUSINESS_ACCOUNT_ID=your_business_account_id
WHATSAPP_ACCESS_TOKEN=your_access_token
WHATSAPP_API_VERSION=v18.0
```

#### Setup
```javascript
const WHATSAPP_API_URL = `https://graph.facebook.com/${process.env.WHATSAPP_API_VERSION}`;
const PHONE_NUMBER_ID = process.env.WHATSAPP_PHONE_NUMBER_ID;
const ACCESS_TOKEN = process.env.WHATSAPP_ACCESS_TOKEN;
```

### Implementation

#### Send Template Message
```javascript
async function sendWhatsAppTemplate({ to, templateName, templateParams }) {
  try {
    const response = await axios.post(
      `${WHATSAPP_API_URL}/${PHONE_NUMBER_ID}/messages`,
      {
        messaging_product: 'whatsapp',
        to: to,
        type: 'template',
        template: {
          name: templateName,
          language: { code: 'en' },
          components: [
            {
              type: 'body',
              parameters: templateParams
            }
          ]
        }
      },
      {
        headers: {
          'Authorization': `Bearer ${ACCESS_TOKEN}`,
          'Content-Type': 'application/json'
        }
      }
    );
    
    return response.data;
  } catch (error) {
    console.error('WhatsApp API error:', error.response?.data);
    throw error;
  }
}
```

### Message Templates

**Order Placed:**
```javascript
await sendWhatsAppTemplate({
  to: customer.phone,
  templateName: 'order_confirmation',
  templateParams: [
    { type: 'text', text: orderNumber },
    { type: 'text', text: vendorName },
    { type: 'text', text: totalAmount }
  ]
});
```

**Order Out for Delivery:**
```javascript
await sendWhatsAppTemplate({
  to: customer.phone,
  templateName: 'order_on_the_way',
  templateParams: [
    { type: 'text', text: orderNumber },
    { type: 'text', text: riderName },
    { type: 'text', text: eta }
  ]
});
```

**Order Delivered:**
```javascript
await sendWhatsAppTemplate({
  to: customer.phone,
  templateName: 'order_delivered',
  templateParams: [
    { type: 'text', text: orderNumber },
    { type: 'text', text: trackingUrl }
  ]
});
```

### Webhook Setup

#### Handle Incoming Messages
```javascript
app.post('/webhooks/whatsapp', async (req, res) => {
  const body = req.body;
  
  if (body.object === 'whatsapp_business_account') {
    body.entry.forEach(entry => {
      entry.changes.forEach(change => {
        if (change.field === 'messages') {
          const message = change.value.messages[0];
          handleIncomingMessage(message);
        }
      });
    });
    
    res.sendStatus(200);
  } else {
    res.sendStatus(404);
  }
});

// Webhook verification
app.get('/webhooks/whatsapp', (req, res) => {
  const mode = req.query['hub.mode'];
  const token = req.query['hub.verify_token'];
  const challenge = req.query['hub.challenge'];
  
  if (mode === 'subscribe' && token === process.env.WHATSAPP_VERIFY_TOKEN) {
    res.status(200).send(challenge);
  } else {
    res.sendStatus(403);
  }
});
```

### Rate Limiting
- **Tier 1:** 1,000 business-initiated conversations per day
- **Higher Tiers:** Request upgrade as volume increases
- Implement queue system for high-volume sending

### Best Practices
- Use approved templates only
- Get user opt-in for promotional messages
- Respond to user messages within 24 hours
- Handle opt-outs properly
- Monitor message delivery status

### Compliance
- GDPR compliance for EU users
- Obtain explicit consent
- Provide opt-out mechanism
- Store consent records
- Respect do-not-disturb hours

---

## 🔐 Security Best Practices

### API Key Management
- Store all keys in environment variables
- Never commit keys to version control
- Rotate keys regularly
- Use different keys for dev/staging/prod
- Monitor API usage for anomalies

### Access Control
- Limit API access to specific IPs (if possible)
- Use IAM roles where applicable
- Implement rate limiting
- Log all API calls
- Alert on unusual patterns

### Data Privacy
- Encrypt sensitive data before sending
- Minimize data sent to third parties
- Comply with data protection regulations
- Regular security audits
- Document data flows

---

## 📊 Monitoring & Alerting

### Key Metrics
- API success/failure rates
- Response times
- Quota usage
- Cost tracking
- Error rates

### Alerts
- API failures > threshold
- Quota approaching limit
- Unusual spikes in usage
- Service degradation
- Cost anomalies

---

## 💰 Cost Management

### Monthly Budget Tracking
| Service | Plan | Monthly Cost | Usage Limits |
|---------|------|--------------|--------------|
| Cloudinary | [Plan] | $X | X GB bandwidth |
| ZeptoMail | [Plan] | $Y | Y emails/month |
| Firebase | [Plan] | $Z | Z notifications |
| WhatsApp API | [Plan] | $W | W conversations |

### Optimization
- Monitor usage trends
- Optimize image sizes
- Batch notifications
- Cache frequently accessed data
- Archive old data

---

## 📚 Related Documentation

- [System Architecture](../architecture/system-architecture.md)
- [Infrastructure](../architecture/infrastructure.md)
- [Notification Service Spec](../services/notification-service-spec.md)
- [API Documentation](../../api-docs/)

---

**Maintained by:** Engineering Team  
**Review Frequency:** Quarterly  
**Last Review:** November 11, 2025

