# Bunny Media Offload - Complete Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [System Requirements](#system-requirements)
3. [Installation](#installation)
4. [Security Configuration](#security-configuration)
5. [Basic Setup](#basic-setup)
6. [Advanced Configuration](#advanced-configuration)
7. [Media Migration](#media-migration)
8. [Image Optimization](#image-optimization)
9. [WPML Multilingual Support](#wpml-multilingual-support)

10. [Troubleshooting](#troubleshooting)
11. [Performance Optimization](#performance-optimization)
12. [Developer Guide](#developer-guide)
13. [FAQ](#faq)

---

## Introduction

Bunny Media Offload is a comprehensive WordPress plugin that seamlessly integrates with Bunny.net Edge Storage to offload, optimize, and deliver your media files through a global CDN network. The plugin provides manual media migration, bulk migration tools, **external image optimization via regional BMO API microservices**, and full WPML multilingual support.

### Workflow Overview

This plugin follows a **manual migration workflow**:
1. **Upload**: Files are uploaded to WordPress normally (can be optimized via external BMO API during upload)
2. **Manual Migration**: Use the admin interface to migrate files to Bunny.net CDN
3. **CDN Delivery**: Once migrated, files are automatically served from CDN

This approach gives you full control over which files are migrated and when.

### Key Features

- **Manual Media Migration**: Upload and optimize media to Bunny.net Edge Storage via admin interface
- **Bulk Migration**: Migrate existing media libraries with animated progress tracking
- **External Image Optimization**: Convert to modern formats AVIF/WebP using high-performance regional BMO API microservices
- **Regional Processing**: Choose between US or EU microservice APIs for compliance and performance
- **Global CDN Delivery**: Serve media from 114+ global edge locations
- **WPML Compatible**: Full multilingual support with shared CDN URLs
- **WooCommerce**: Seamless product image handling with High-Performance Order Storage support

- **Security First**: wp-config.php configuration support for sensitive data

---

## System Requirements

### Minimum Requirements
- **WordPress**: 5.0 or higher
- **PHP**: 7.4 or higher (8.0+ recommended)
- **Memory**: 128MB minimum, 256MB recommended
- **cURL**: Required for API communication
- **GD Library**: Required for local image processing (if external optimization is disabled)

### Recommended Environment
- **WordPress**: 6.0+
- **PHP**: 8.1+
- **Memory**: 512MB+
- **WooCommerce**: 5.0+ (if using e-commerce features)
- **WPML**: 4.0+ (if using multilingual features)

### API Requirements
- Active Bunny.net account with Edge Storage zone configured
- **BMO API Key**: Required for external image optimization (set via wp-config.php)
- **Regional API Selection**: Choose US or EU microservice based on your location/compliance needs

---

## Installation

### Automatic Installation (WordPress Repository)
1. Log in to your WordPress admin panel
2. Navigate to **Plugins > Add New**
3. Search for "Bunny Media Offload"
4. Click **Install Now** and then **Activate**

### Manual Installation
1. Download the plugin files
2. Upload the `bunny-media-offload` folder to `/wp-content/plugins/`
3. Activate the plugin through **Plugins > Installed Plugins**
4. Navigate to **Bunny CDN** in the admin menu

---

## Configuration System

The plugin uses a hybrid configuration system:
- **API credentials** are stored in `wp-config.php` for security
- **All other settings** are stored in a JSON configuration file for portability and performance

### wp-config.php Constants (Required)

Add **these constants** to your `wp-config.php` file, **before** the `/* That's all, stop editing! */` line:

```php
<?php
// Bunny.net Edge Storage Configuration
// Only these three constants should be defined in wp-config.php

// Required: Your Bunny.net Storage API Key
define('BUNNY_API_KEY', 'your-storage-api-key-here');

// Required: Your Bunny.net Storage Zone Name
define('BUNNY_STORAGE_ZONE', 'your-storage-zone-name');

// Required: Custom hostname for CDN URLs (without https://)
define('BUNNY_CUSTOM_HOSTNAME', 'cdn.yoursite.com');

// BMO API Configuration (Required for Image Optimization)
define('BMO_API_KEY', 'your-bmo-api-key-here');
define('BMO_API_REGION', 'us'); // 'us' or 'eu'
```

### JSON Configuration File

All other settings are automatically managed in `/wp-content/bunny-config.json`. This file is created automatically with defaults when the plugin is first activated.

### Example Complete wp-config.php Section

```php
<?php

// ** Bunny.net Configuration ** //
define('BUNNY_API_KEY', 'b8f2c4d5-1234-5678-9abc-def123456789');
define('BUNNY_STORAGE_ZONE', 'mysite-storage');
define('BUNNY_CUSTOM_HOSTNAME', 'cdn.mysite.com');

// ** BMO API Configuration ** //
define('BMO_API_KEY', 'your-bmo-api-key-here');
define('BMO_API_REGION', 'us'); // 'us' or 'eu'

/* That's all, stop editing! Happy publishing. */
require_once ABSPATH . 'wp-settings.php';
```

### Benefits of This Configuration System

1. **Enhanced Security**: API credentials stored in `wp-config.php`, not in database or JSON file
2. **Environment Portability**: 
   - Credentials in `wp-config.php` for environment-specific settings
   - Configuration in JSON file for consistent application settings
3. **Regional Compliance**: EU region API for GDPR compliance requirements
4. **Version Control Safe**: 
   - Exclude `wp-config.php` from commits (credentials)
   - Include `bunny-config.json` in version control (shared settings)
5. **Performance**: JSON file loads faster than database queries, regional APIs reduce latency
6. **Backup Safety**: Settings preserved during database restores
7. **Easy Management**: Modify JSON file directly or use admin interface

---

## Basic Setup

### Step 1: Bunny.net Account Setup

1. **Create Account**: Sign up at [bunny.net](https://bunny.net)
2. **Create Storage Zone**:
   - Navigate to **Storage > Storage Zones**
   - Click **Add Storage Zone**
   - Enter a name (e.g., "mysite-media")
   - Select your preferred region
   - Note the storage zone name for configuration

3. **Generate API Key**:
   - Go to **Account > API**
   - Create a new API key with **Storage** permissions
   - Copy the API key securely

4. **Custom Hostname (Required)**:
   - Navigate to **Storage > Storage Zones**
   - Click on your storage zone
   - Go to **Custom Hostnames**
   - Add your custom domain (e.g., cdn.yoursite.com)
   - Configure DNS CNAME record pointing to your storage zone
   - **Important**: Custom hostname is mandatory for this plugin to function

### Step 2: Plugin Configuration

#### Option A: Using wp-config.php (Recommended)
Add the constants to your `wp-config.php` file as shown in the [Security Configuration](#security-configuration) section.

#### Option B: Using Admin Interface
1. Navigate to **Bunny CDN > Settings**
2. Enter your API key and storage zone name
3. Configure optional settings
4. Click **Test Connection** to verify setup

### Step 3: Test Connection

1. Go to **Bunny CDN > Dashboard**
2. Click **Test Connection**
3. Verify you see a success message
4. Check that the connection indicator shows green

---

## Advanced Configuration

### File Type Support

This plugin supports modern file formats for optimal performance:

- **WebP**: Modern image format with excellent compression and wide browser support
- **AVIF**: Next-generation image format with superior compression ratios
- **SVG**: Scalable vector graphics for icons and simple graphics

> **Important**: WebP, AVIF, and SVG file formats are supported for migration and synchronization. Only files exceeding the configured size threshold are migrated to ensure efficient storage usage.

### Post Type Filtering

Limit offloading to specific post types:

```php
// In wp-config.php  
define('BUNNY_ALLOWED_POST_TYPES', 'post,page,product');
```

### Custom Upload Paths

Organize files with custom directory structures:

```php
// Custom filter in your theme's functions.php
add_filter('bunny_remote_path', function($path, $attachment_id) {
    $post = get_post($attachment_id);
    $year = date('Y', strtotime($post->post_date));
    $month = date('m', strtotime($post->post_date));
    
    return "uploads/{$year}/{$month}/" . basename($path);
}, 10, 2);
```

### CDN URL Customization

Modify CDN URLs for specific use cases:

```php
// Force HTTPS and add custom parameters
add_filter('bunny_cdn_url', function($url, $attachment_id) {
    $url = str_replace('http://', 'https://', $url);
    return $url . '?quality=85&format=auto';
}, 10, 2);
```

---

## Media Migration

### Planning Your Migration

Before starting a bulk migration:

1. **Backup Your Site**: Create a full site backup
2. **Test with Small Batch**: Start with 10-20 files
3. **Check Available Storage**: Ensure sufficient Bunny.net storage quota
4. **Plan Downtime**: Large migrations may impact site performance

### Starting Migration

#### Via Admin Interface
1. Navigate to **Bunny CDN > Migration**
2. Supported file types to migrate:
   - **WebP Images**: Modern image format with excellent compression
   - **AVIF Images**: Next-generation image format with superior compression
   - **SVG Images**: Scalable vector graphics for icons and simple graphics
   
   > **Note**: WebP, AVIF, and SVG formats are supported for migration. Only files exceeding the configured size threshold are migrated.
3. Choose batch size (recommended: 50-100 files)
4. For WPML sites, select language scope
5. Click **Start Migration**

### Monitoring Migration Progress

The migration interface provides real-time updates:
- **Total Files**: Number of files to process
- **Processed**: Files completed (successful + failed)
- **Success Rate**: Percentage of successful uploads
- **Current Batch**: Files in current processing batch
- **Estimated Time**: Remaining time based on current speed

### Handling Migration Issues

#### Common Issues and Solutions

**Migration Stalls:**
- Check migration status via admin interface: **Bunny CDN > Migration**
- Cancel current migration and restart with smaller batch size
- Monitor server logs for errors

**File Permission Errors:**
```bash
# Check file permissions
ls -la wp-content/uploads/

# Fix permissions if needed
chmod 644 wp-content/uploads/2024/01/*
```

**Memory Issues:**
```php
// In wp-config.php - increase memory limit
ini_set('memory_limit', '512M');
define('WP_MEMORY_LIMIT', '512M');
```

---

## Image Optimization

### New External Optimization System

The plugin now uses **external regional BMO API microservices** for high-performance image optimization:

#### Regional Microservice APIs
- **US Region**: `https://api-us.bmo.nexwinds.com/v1/images/wp/optimize`
- **EU Region**: `https://api-eu.bmo.nexwinds.com/v1/images/wp/optimize` (GDPR Compliant)

#### Key Features
- **Batch Processing**: Fixed batch size of 10 images per API request
- **Modern Formats**: AVIF and WebP with intelligent format selection
- **Smart Optimization**: Content-aware quality optimization
- **Regional Processing**: Choose US or EU based on compliance needs
- **Enhanced Performance**: Dedicated Fastify microservices with auto-scaling

### Optimization Settings

#### Regional Configuration
Set your preferred region in `wp-config.php`:
```php
define('BMO_API_REGION', 'us'); // or 'eu'
```

#### Format Selection
- **AVIF**: Best compression, newest format, growing browser support
- **WebP**: Good compression, wide browser support
- **Auto**: API intelligently selects format based on browser support

#### Batch Processing
- **Fixed Batch Size**: 10 images per API request (no longer user-configurable)
- **Concurrent Processing**: Images are processed externally, no local concurrent limits
- **Queue Management**: Automatic queue management for large optimization tasks

### Optimization Configuration

```php
// In wp-config.php
define('BMO_API_KEY', 'your-bmo-api-key');
define('BMO_API_REGION', 'us'); // or 'eu'

define('BUNNY_OPTIMIZATION_FORMAT', 'auto'); // 'avif', 'webp', or 'auto'
define('BUNNY_OPTIMIZATION_QUALITY', 85); // 1-100
```

### API Request Format

The plugin automatically formats requests to the BMO API:

```json
{
  "images": [
    {
      "imageUrl": "https://example.com/image.jpg",
      "quality": 85
    }
  ],
  "supportsAVIF": true,
  "batch": true
}
```

### Optimization Results

The BMO API returns comprehensive optimization data:
- **Original Format**: Input image format
- **Target Format**: Optimized output format (AVIF/WebP)
- **Compression Ratio**: Percentage size reduction
- **Processing Time**: Optimization duration
- **Credits Used**: API credits consumed

### Migration from Local Processing

**Important Changes:**
- ~~**Optimization Concurrent Limit**: Removed (processing now external)~~
- **Batch Size**: Fixed at 10 images (no longer user-configurable)
- **Processing Location**: External BMO API microservices
- **Regional Selection**: New requirement to choose US or EU API

---

## WPML Multilingual Support

### Setup Requirements

1. **Install WPML**: Core, String Translation, and Media Translation
2. **Configure Languages**: Set up your site's languages
3. **Enable Media Translation**: Go to **WPML > Settings > Media Translation**

### WPML Integration Features

When WPML is detected, the plugin:
- Shares CDN URLs across all language versions
- Synchronizes migration metadata when translations are created  
- Prevents duplicate optimization of the same physical file
- Provides language-specific migration options

### Migration with WPML

#### Language Scope Options

**Current Language Only:**
- Migrates only files attached to content in the current language
- Useful for gradual, language-by-language migration
- Reduces processing load

**All Languages:**
- Migrates files from all active languages
- Ensures complete coverage
- May include duplicate files across languages

### Optimization with WPML

The plugin optimizes efficiently across languages:
- Original files optimized once
- All language versions share the optimized file
- Metadata synchronized across translations
- No redundant processing

---

## Troubleshooting

### Connection Issues

#### API Key Problems
**Symptoms**: "Invalid API key" or "Authorization failed"

**Solutions**:
```bash
# Test API key directly
curl -H "AccessKey: YOUR_API_KEY" \
     https://storage.bunnycdn.com/YOUR_ZONE/

# Verify API key has storage permissions in Bunny.net dashboard
# Regenerate API key if necessary
```

#### Network Connectivity
**Symptoms**: "Connection timeout" or "Could not resolve host"

**Solutions**:
```bash
# Test connectivity from server
curl -I https://storage.bunnycdn.com/

# Check firewall rules allow outbound HTTPS
# Verify DNS resolution works

# Test with specific storage zone
curl -I https://YOUR_ZONE.b-cdn.net/
```

### Upload Failures

#### File Permission Issues
**Symptoms**: "Permission denied" or "Could not read file"

**Solutions**:
```bash
# Check file permissions
ls -la wp-content/uploads/2024/01/

# Fix permissions
find wp-content/uploads/ -type f -exec chmod 644 {} \;
find wp-content/uploads/ -type d -exec chmod 755 {} \;

# Check PHP file upload limits
php -i | grep -E "(upload_max_filesize|post_max_size|max_execution_time)"
```

#### Storage Quota Exceeded
**Symptoms**: "Storage quota exceeded" or "Insufficient storage"

**Solutions**:
1. Check storage usage in Bunny.net dashboard
2. Upgrade storage plan if needed
3. Clean up unnecessary files
4. Implement file retention policies

### Migration Problems

#### Stalled Migration
**Symptoms**: Migration progress stops advancing

**Solutions**:
- Check migration status via **Bunny CDN > Migration** page
- Cancel current migration via admin interface
- Restart with smaller batch size (10-25 files)
- Check server logs for specific errors:
  ```bash
  tail -f wp-content/debug.log
  ```

#### Memory Issues
**Symptoms**: "Fatal error: Allowed memory size exhausted"

**Solutions**:
```php
// In wp-config.php
ini_set('memory_limit', '512M');
define('WP_MEMORY_LIMIT', '512M');
```

### Performance Issues

#### Slow Migration
**Symptoms**: Migration takes much longer than expected

**Solutions**:
1. Reduce batch size: 10-25 files per batch
2. Increase PHP max_execution_time
3. Run migrations during low-traffic periods
4. Use admin interface for better control and monitoring

#### High Server Load
**Symptoms**: Website becomes slow during operations

**Solutions**:
1. Schedule operations during maintenance windows
2. Use smaller batch sizes
3. Implement rate limiting:
   ```php
   // Add delay between operations
   add_filter('bunny_operation_delay', function() {
       return 2; // 2 second delay
   });
   ```

### File Integrity Issues

#### Missing Files
**Symptoms**: "File not found" errors on frontend

**Solutions**:
- Use **Bunny CDN > Sync & Recovery** page to verify file integrity
- Download missing files via **Sync** functionality
- Re-upload files via **Migration** page

#### Corrupted Files
**Symptoms**: Images appear broken or incomplete

**Solutions**:
- Use **Bunny CDN > Sync & Recovery** page for integrity verification
- Re-upload corrupted files via **Migration** page
- Check file integrity via admin interface

### Optimization Issues

#### Optimization Failures
**Symptoms**: Images not being optimized or errors in logs

**Solutions**:
```bash
# Check GD library support
php -m | grep -i gd
```
- Verify optimization status via **Bunny CDN > Optimization** page
- Clear optimization queue via admin interface
- Re-run optimization through admin panel

#### Quality Issues
**Symptoms**: Optimized images appear too compressed

**Solutions**:
1. Increase target file size (55KB or 60KB)
2. Switch to WebP format for better quality
3. Disable optimization for specific file types:
   ```php
   add_filter('bunny_skip_optimization', function($skip, $attachment_id) {
       $file = get_attached_file($attachment_id);
       // Skip PNG files
       return pathinfo($file, PATHINFO_EXTENSION) === 'png';
   }, 10, 2);
   ```

### WPML Issues

#### Translation Sync Problems
**Symptoms**: Translated attachments missing CDN URLs

**Solutions**:
- Check WPML media translation settings in **WPML > Settings**
- Re-sync translated attachments via **Bunny CDN > Sync & Recovery**
- Verify WPML configuration in plugin settings

### Debugging

#### Enable Debug Logging
```php
// In wp-config.php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('BUNNY_DEBUG', true);
```

#### Useful Log Monitoring
```bash
# Monitor WordPress debug log
tail -f wp-content/debug.log
```
- Monitor plugin-specific logs via **Bunny CDN > Logs** page
- Export logs for support via admin interface

---

## Performance Optimization

### Server Configuration

#### PHP Settings
```php
// Recommended php.ini settings
memory_limit = 512M
max_execution_time = 300
upload_max_filesize = 100M
post_max_size = 100M
max_input_time = 300
```

#### WordPress Configuration
```php
// In wp-config.php
define('WP_MEMORY_LIMIT', '512M');
define('WP_MAX_MEMORY_LIMIT', '512M');

// Increase cron timeout for background operations
define('WP_CRON_LOCK_TIMEOUT', 300);
```

### CDN Optimization

#### Cache Headers
Configure appropriate cache headers in Bunny.net:
- **Images**: 1 year (31536000 seconds)
- **Videos**: 1 month (2592000 seconds) 
- **Documents**: 1 week (604800 seconds)

#### Compression
Enable gzip compression in your Bunny.net pull zone:
1. Go to **Pull Zones > Your Zone > Edge Rules**
2. Add rule: **Enable Gzip Compression**
3. Apply to all file types

### Database Optimization

#### Regular Maintenance
```bash
# Optimize plugin tables
wp db optimize
```
- Clean old logs via **Bunny CDN > Logs** page
- Statistics are automatically maintained

#### Index Optimization
The plugin automatically creates optimal database indexes, but you can verify:
```sql
SHOW INDEX FROM wp_bunny_offloaded_files;
SHOW INDEX FROM wp_bunny_optimization_queue;
```

### Monitoring Performance

#### Built-in Statistics
Monitor performance via **Bunny CDN > Dashboard**:
- Files offloaded per day/week/month
- Storage savings percentage
- Bandwidth usage reduction
- Optimization compression ratios

All statistics and performance metrics are available via the **Bunny CDN > Dashboard** page in the WordPress admin.

---

## Developer Guide

### Architecture Overview

The plugin follows WordPress best practices with a modular architecture:

```
Bunny_Media_Offload (Main Controller)
├── Bunny_API (External API Communication)
├── Bunny_Uploader (File Upload Handling)
├── Bunny_Migration (Bulk Operations)
├── Bunny_Optimizer (Image Optimization)
├── Bunny_Sync (File Synchronization)
├── Bunny_Admin (WordPress Admin Interface)
├── Bunny_Settings (Configuration Management)
├── Bunny_Stats (Statistics and Analytics)
├── Bunny_Logger (Logging and Debugging)
├── Bunny_WPML (Multilingual Support)
└── Bunny_Utils (Helper Functions)
```

### Hooks and Filters

#### Upload Hooks
```php
// Modify upload behavior
add_filter('bunny_before_upload', function($file_path, $attachment_id) {
    // Custom logic before upload
    return $file_path;
}, 10, 2);

// Post-upload processing
add_action('bunny_after_upload', function($attachment_id, $bunny_url) {
    // Custom logic after successful upload
}, 10, 2);

// Upload failure handling
add_action('bunny_upload_failed', function($attachment_id, $error) {
    // Custom error handling
}, 10, 2);
```

#### URL Filtering
```php
// Customize CDN URLs
add_filter('bunny_cdn_url', function($url, $attachment_id) {
    // Add custom parameters
    return $url . '?watermark=true';
}, 10, 2);

// Conditional URL modification
add_filter('bunny_cdn_url', function($url, $attachment_id) {
    $post = get_post($attachment_id);
    if ($post && $post->post_type === 'product') {
        // Use different CDN for product images
        return str_replace('cdn.site.com', 'products.cdn.site.com', $url);
    }
    return $url;
}, 10, 2);
```

#### Optimization Hooks
```php
// Skip optimization for specific files
add_filter('bunny_skip_optimization', function($skip, $attachment_id) {
    $meta = wp_get_attachment_metadata($attachment_id);
    // Skip files larger than 5MB
    return $meta && $meta['filesize'] > 5242880;
}, 10, 2);

// Custom optimization settings per file
add_filter('bunny_optimization_settings', function($settings, $attachment_id) {
    $post = get_post($attachment_id);
    if ($post && has_term('high-quality', 'attachment_category', $post)) {
        $settings['max_size'] = 100; // Less compression for high-quality images
    }
    return $settings;
}, 10, 2);
```

#### Migration Hooks
```php
// Custom migration filters
add_filter('bunny_migration_query', function($query) {
    // Only migrate images from the last year
    global $wpdb;
    $query .= $wpdb->prepare(
        " AND post_date > %s", 
        date('Y-m-d', strtotime('-1 year'))
    );
    return $query;
});

// Pre-migration validation
add_filter('bunny_before_migration', function($attachment_ids) {
    // Remove attachments that shouldn't be migrated
    return array_filter($attachment_ids, function($id) {
        return !get_post_meta($id, '_skip_bunny_migration', true);
    });
});
```

### Custom Extensions

#### Creating a Custom Module
```php
class My_Bunny_Extension {
    private $bunny;
    
    public function __construct() {
        add_action('bunny_loaded', array($this, 'init'));
    }
    
    public function init($bunny_instance) {
        $this->bunny = $bunny_instance;
        $this->add_hooks();
    }
    
    private function add_hooks() {
        add_filter('bunny_cdn_url', array($this, 'modify_urls'), 20, 2);
        add_action('bunny_after_upload', array($this, 'post_upload'), 10, 2);
    }
    
    public function modify_urls($url, $attachment_id) {
        // Custom URL logic
        return $url;
    }
    
    public function post_upload($attachment_id, $bunny_url) {
        // Post-upload logic
    }
}

new My_Bunny_Extension();
```

#### Database Schema Extensions

Add custom tables for extensions:
```php
function my_bunny_create_tables() {
    global $wpdb;
    
    $table_name = $wpdb->prefix . 'my_bunny_extension';
    
    $charset_collate = $wpdb->get_charset_collate();
    
    $sql = "CREATE TABLE $table_name (
        id int(11) NOT NULL AUTO_INCREMENT,
        attachment_id int(11) NOT NULL,
        custom_data longtext,
        created_at datetime DEFAULT CURRENT_TIMESTAMP,
        PRIMARY KEY (id),
        KEY attachment_id (attachment_id)
    ) $charset_collate;";
    
    require_once(ABSPATH . 'wp-admin/includes/upgrade.php');
    dbDelta($sql);
}

register_activation_hook(__FILE__, 'my_bunny_create_tables');
```

### API Integration Examples

#### Custom API Endpoints
```php
add_action('rest_api_init', function() {
    register_rest_route('bunny/v1', '/migrate', array(
        'methods' => 'POST',
        'callback' => 'my_custom_migration',
        'permission_callback' => function() {
            return current_user_can('manage_options');
        }
    ));
});

function my_custom_migration($request) {
    $bunny = Bunny_Media_Offload::get_instance();
    
    // Custom migration logic
    $result = $bunny->migration->start_custom_migration($request->get_params());
    
    return new WP_REST_Response($result, 200);
}
```

#### External Service Integration
```php
// Integrate with external image processing service
add_action('bunny_before_upload', function($file_path, $attachment_id) {
    if (wp_attachment_is_image($attachment_id)) {
        // Send to external processing service
        $processed_path = my_external_processor($file_path);
        return $processed_path;
    }
    return $file_path;
}, 5, 2); // Priority 5 to run before optimization
```

---

## FAQ

### General Questions

**Q: Is my data safe with Bunny.net?**
A: Yes, Bunny.net provides enterprise-grade security with SSL encryption, DDoS protection, and SOC 2 compliance. Your media files are distributed across multiple data centers for redundancy.

**Q: Will this plugin slow down my WordPress site?**
A: No, the plugin is designed for performance. Only migrated files are served from CDN, improving load times. The manual migration process may temporarily impact performance during bulk operations.

**Q: Can I use this with other CDN plugins?**
A: It's not recommended to use multiple CDN plugins simultaneously as they may conflict. Disable other CDN plugins before using Bunny Media Offload.

**Q: What happens if I deactivate the plugin?**
A: Your media files remain on Bunny.net, but WordPress will revert to looking for local files. You can re-download files using the sync feature before deactivation.

**Q: Do new uploads automatically go to the CDN?**
A: No, this plugin uses a manual migration workflow. New uploads stay on your server until you manually migrate them via **Bunny CDN > Migration**. This gives you control over which files are migrated.

### Technical Questions

**Q: Do I need to modify my theme?**
A: No, the plugin automatically handles URL rewriting. Your theme's image display code remains unchanged.

**Q: Can I migrate existing media files?**
A: Yes, use the bulk migration tool in **Bunny CDN > Migration**.

**Q: How does the plugin handle image sizes (thumbnails)?**
A: WordPress thumbnail generation works normally. The plugin uploads all image sizes to Bunny.net and serves them via CDN.

**Q: Can I use custom image transformations?**
A: Yes, if you have Bunny.net's Image Optimizer enabled, you can use URL parameters for transformations:
```
https://cdn.yoursite.com/image.jpg?width=300&height=200&quality=85
```

### Pricing and Costs

**Q: How much does Bunny.net storage cost?**
A: Bunny.net charges approximately $0.01/GB/month for storage plus $0.01/GB for bandwidth. Most WordPress sites see significant cost savings compared to traditional hosting.

**Q: Are there any hidden fees?**
A: No, Bunny.net uses transparent pay-as-you-go pricing. You only pay for storage used and bandwidth consumed.

**Q: How can I estimate my costs?**
A: Use the cost calculator on Bunny.net or check your current storage usage in **Bunny CDN > Dashboard** after installation.

### Troubleshooting

**Q: Why do I see "broken image" icons?**
A: This usually indicates:
1. Incorrect API key or storage zone configuration
2. Files not properly uploaded to Bunny.net
3. DNS issues with custom hostname

Run **Test Connection** in settings and check the logs.

**Q: Can I restore my local files?**
A: Yes, use the sync feature via **Bunny CDN > Sync & Recovery** to download files back from Bunny.net.

**Q: How do I handle SSL certificate issues?**
A: Ensure your custom hostname has a valid SSL certificate. Bunny.net provides free SSL certificates for custom hostnames.

### Migration Questions

**Q: How long does migration take?**
A: Migration time depends on:
- Number of files
- Total file size
- Server performance
- Network speed

Typical rates: 50-100 files per minute for images.

**Q: Can I pause and resume migration?**
A: Yes, migrations can be paused and resumed. Progress is saved automatically.

**Q: What if migration fails partway through?**
A: The plugin tracks progress and only processes remaining files when restarted. Check logs for specific error details.

### WPML Questions

**Q: Do I need separate storage zones for each language?**
A: No, all languages can share the same storage zone and CDN URLs, reducing costs.

**Q: How does media translation work?**
A: When WPML creates translated attachments, the plugin automatically links them to the same CDN file, preventing duplicate uploads.

**Q: Can I migrate specific languages only?**
A: Yes, the migration tool offers "Current Language Only" and "All Languages" options.

---

## Support and Resources

### Getting Help

1. **Documentation**: This comprehensive guide covers most use cases
2. **WordPress Support Forums**: Community support and discussions
3. **Plugin Logs**: Check **Bunny CDN > Logs** for error details
4. **Plugin Logs**: Check **Bunny CDN > Logs** for detailed information

### Useful Resources

- **Bunny.net Documentation**: [docs.bunny.net](https://docs.bunny.net)
- **WordPress Developer Reference**: [developer.wordpress.org](https://developer.wordpress.org)

- **WPML Documentation**: [wpml.org/documentation](https://wpml.org/documentation)

### Reporting Issues

When reporting issues, please include:
1. WordPress and PHP versions
2. Plugin version
3. Error messages from logs
4. Steps to reproduce the issue
5. Server configuration details

### Contributing

The plugin is open source and welcomes contributions:
- Report bugs and feature requests
- Submit translations
- Contribute code improvements
- Help with documentation

---

*Last updated: June 2025*
*Plugin version: 1.0.0* 