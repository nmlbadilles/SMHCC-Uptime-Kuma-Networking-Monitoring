# SMHCC Uptime Kuma Network Monitoring

This repository outlines the network monitoring setup for SMHCC properties using [Uptime Kuma](https://github.com/louislam/uptime-kuma) with Google Sheets as a live dashboard for tracking network device statuses.

##SMHCC Network Monitoring Dashboard

https://docs.google.com/spreadsheets/d/1_P0oMS6h8NoWAyyC8--FHWaPzNFSe1JSX3jLbWmEcSw/edit?usp=sharing

## Technology Overview

- **Monitoring Tool**: Uptime Kuma
- **Notifications**: Webhook notifications from Uptime Kuma
- **Database**: Google Sheets
- **Dashboard**: The latest network device status is displayed on a Google Sheets dashboard, fed by webhook updates from Uptime Kuma.

## Usage Instructions

### Adding a Monitor and Status Cells
To monitor additional devices, simply add new cells to the designated monitoring area in your Google Sheets file. Use the formula provided below to track the status (Up/Down) of each network device.

#### Formula to Track Monitor Status:
For each monitor, use the following formula in the relevant cell to display the status as either "✅ Up" or "🔴 Down":

```excel
=IF(ISNUMBER(SEARCH("up", INDEX(FILTER(<HOTELCODE>!C:C, ISNUMBER(SEARCH("<MONITOR NAME>", <HOTELCODE>!C:C))), COUNTA(FILTER(<HOTELCODE>!C:C, ISNUMBER(SEARCH("<MONITOR NAME>",<HOTELCODE>!C:C))))))), "✅ Up", "🔴 Down")
```

Replace `<HOTELCODE>` with the sheet name for the respective hotel.  
Replace `<MONITOR NAME>` with the name of the monitor/device you want to track.  
When adding new rows or columns for new monitors, make sure to shift existing cells up or down accordingly to maintain the correct structure.

### Setting Up Webhooks
To set up webhook notifications for new devices, follow the instructions in the **GUIDE** sheet within the workbook. The guide provides step-by-step instructions for integrating Uptime Kuma's webhook into the Google Sheets dashboard.

## Sheet Structure

The Google Sheets dashboard is structured as follows:

- **<HOTELCODE> Sheets**: Each hotel property has its own (hidden) sheet, identified by the hotel's code, where the Uptime Kuma logs are stored.
- **GUIDE Sheet**: Provides detailed instructions on how to add new monitors and configure webhook notifications.

## How It Works

1. **Uptime Kuma** monitors the network devices' status.
2. When a device goes offline or comes online, a webhook notification is triggered and sent to Google Sheets.
3. Google Sheets parses the webhook data and updates the corresponding device’s status on the dashboard.
4. The status is displayed as either "✅ Up" (if the device is online) or "🔴 Down" (if the device is offline).

## Troubleshooting

### Common Issues

- **Formula Not Returning the Correct Status**: Double-check that the `<MONITOR NAME>` exactly matches the name in Uptime Kuma and ensure that the webhook is properly set up.
- **Webhook Not Triggering**: Ensure that Uptime Kuma is configured to send webhook notifications and that the Google Sheets webhook URL is correctly entered.

## Additional Information

For any additional details or questions, refer to the **GUIDE** sheet within the workbook or reach out to the network administrator for support.

## Contribution

This repository follows SMHCC guidelines. Contributions are welcome, but please ensure that any proposed changes align with the system requirements and overall setup. To propose changes, open an issue or submit a pull request.

## License

This project is licensed under the terms of the [MIT License](LICENSE).
