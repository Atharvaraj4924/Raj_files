# MongoDB Atlas Setup Guide for Dr. Desai Website

## Overview

Your vitals functionality is already fully implemented and ready to connect to MongoDB Atlas. The system uses the `MedicalRecord` model to store patient vitals data with the following endpoints:

- `PUT /api/medical-records/vitals/:patientId` - Update patient vitals
- `GET /api/medical-records/vitals/:patientId` - Get patient vitals history

## Step 1: Create MongoDB Atlas Account

1. Go to [MongoDB Atlas](https://www.mongodb.com/atlas)
2. Sign up for a free account
3. Create a new project (e.g., "Dr. Desai Website")

## Step 2: Create a Cluster

1. Click "Build a Database"
2. Choose "FREE" tier (M0 Sandbox)
3. Select a cloud provider and region close to you
4. Name your cluster (e.g., "dr-desai-cluster")
5. Click "Create Cluster"

## Step 3: Create Database User

1. Go to "Database Access" in the left sidebar
2. Click "Add New Database User"
3. Choose "Password" authentication
4. Create a username and strong password
5. Set privileges to "Read and write to any database"
6. Click "Add User"

## Step 4: Whitelist Your IP Address

1. Go to "Network Access" in the left sidebar
2. Click "Add IP Address"
3. For development, click "Allow Access from Anywhere" (0.0.0.0/0)
4. For production, add your specific IP addresses
5. Click "Confirm"

## Step 5: Get Connection String

1. Go to "Clusters" in the left sidebar
2. Click "Connect" on your cluster
3. Choose "Connect your application"
4. Select "Node.js" and version "4.1 or later"
5. Copy the connection string

## Step 6: Configure Environment Variables

Create a `.env` file in the `server` directory:

```env
# MongoDB Atlas Connection
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster-url>/dr_desai_appointments?retryWrites=true&w=majority

# JWT Secret (generate a strong secret)
JWT_SECRET=your-super-secret-jwt-key-here-make-it-long-and-random

# Server Configuration
PORT=5000
NODE_ENV=development

# CORS Origins
CORS_ORIGINS=http://localhost:3000,http://localhost:5173
```

**Important**: Replace `<username>`, `<password>`, and `<cluster-url>` with your actual MongoDB Atlas credentials.

## Step 7: Test the Connection

Run the connection test:

```bash
cd server
node test-connection.js
```

You should see:
```
✅ MongoDB Connected: <cluster-url>
📊 Database: dr_desai_appointments
📋 Medical Records in database: 0
🔌 Connection closed
```

## Step 8: Start the Server

```bash
cd server
npm run dev
```

## Vitals Data Structure

The system stores vitals data in the following format:

```javascript
{
  patient: ObjectId, // Reference to User
  doctor: ObjectId,  // Reference to User (optional)
  vitals: {
    weight: { value: 70, unit: 'kg', date: Date },
    height: { value: 175, unit: 'cm', date: Date },
    heartRate: { value: 72, unit: 'bpm', date: Date },
    bloodPressure: { systolic: 120, diastolic: 80, date: Date },
    temperature: { value: 36.5, unit: '°C', date: Date }
  },
  createdAt: Date,
  updatedAt: Date
}
```

## Frontend Integration

The frontend is already configured to work with these endpoints:

1. **PatientVitals.jsx** - Displays vitals form and history
2. **API calls** - Automatically handle authentication and data formatting
3. **Real-time updates** - Uses React Query for caching and updates

## Troubleshooting

### Connection Issues

1. **Authentication failed**: Check username/password in connection string
2. **Network timeout**: Check IP whitelist in MongoDB Atlas
3. **Database not found**: The database will be created automatically on first use

### Vitals Not Saving

1. Check browser console for API errors
2. Verify JWT token is present in localStorage
3. Check server logs for validation errors

### Environment Variables

Make sure your `.env` file is in the `server` directory and contains all required variables.

## Security Notes

1. Never commit your `.env` file to version control
2. Use strong, unique passwords for database users
3. Restrict IP access in production
4. Use environment-specific JWT secrets

## Next Steps

Once connected, your vitals functionality will:
- ✅ Store patient vitals in MongoDB Atlas
- ✅ Display vitals history with timestamps
- ✅ Allow patients to update their own vitals
- ✅ Provide real-time data updates
- ✅ Maintain data integrity with validation

The system is production-ready and will scale with your MongoDB Atlas cluster.
