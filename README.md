Bug Fixes Documentation – Supabase Functions & Lead Capture
Overview
This document outlines key fixes applied to the Supabase function deployment, the lead capture form, and the email personalization function.

Critical Fixes Implemented
1. Config.toml Deployment Errors
File: config.toml
Severity: Critical
Status: ✅ Fixed

Problem
auth.sms.test_otp was a plain string instead of the required map format, causing parsing errors.

The [[edge_runtime.policies]] block was invalid and blocked function deployment.

Root Cause
The config expected a map for test_otp, but received a string.

edge_runtime.policies is not a supported key in the current config schema.

Fix
Changed test_otp to a map with phone numbers as keys, e.g.:

toml
Copy
Edit
[auth.sms.test_otp]
"+15555555555" = "123456"
Removed the entire [[edge_runtime.policies]] block.

Impact
✅ Config parses correctly

✅ Functions deploy without errors

2. Duplicate Function Invocation in Lead Capture Form
File: LeadCaptureForm.tsx
Severity: Medium
Status: ✅ Fixed

Problem
The supabase.functions.invoke('send-confirmation', ...) was called twice sequentially on form submission, resulting in redundant API calls and potential duplicate emails.

Root Cause
A copy-paste mistake caused the same function call to appear twice with no difference.

Fix
Removed the second, redundant call to supabase.functions.invoke to ensure only a single email is sent per submission.

Impact
✅ Eliminates duplicate emails

✅ Simplifies code and improves efficiency

✅ No change to user experience

3. AI Email Generation Code Update
File: send-confirmation function (Deno)
Severity: Medium
Status: ✅ Fixed

Problem
The AI response was incorrectly accessed at choices[1], which caused errors or undefined content.

Root Cause
OpenAI API responses use choices[0] for the primary message, not choices[1].

Fix
Updated to use choices[0] for email content.

Added fallback static content in case AI generation fails.

Impact
✅ Personalized emails generate correctly

✅ Reliable fallback prevents broken emails

✅ Improved logging for debugging

