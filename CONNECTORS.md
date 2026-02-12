# Connectors

## How customization points work

Plugin files use `~~category` as a placeholder for values that vary by organization. During plugin setup (via the **Manage** button), these placeholders are replaced with your company's actual values.

## Connectors for this plugin

| Category | Placeholder | Default | Description |
|----------|-------------|---------|-------------|
| Currency | `~~currency` | AED | Currency code used for all pricing and financial outputs |
| Tax name | `~~tax_name` | VAT | Name of the applicable tax (VAT, GST, Sales Tax, etc.) |
| Tax rate | `~~tax_rate` | 5% | Tax rate applied to proposal totals |
| Tax authority | `~~tax_authority` | UAE Federal Tax Authority | The tax authority that governs the rate |
| Region | `~~region` | GCC | Geographic region for experience requirements and qualification standards |
| Classification authority | `~~classification_authority` | ADM | Local authority for company classification and compliance (e.g., ADM, Dubai Municipality, Ashghal, MOMRAH) |
| Working hours/month | `~~working_hours_per_month` | 176 | Standard billable hours per month for rate conversion |
| Working days/month | `~~working_days_per_month` | 22 | Standard working days per month |
| Third-party markup | `~~third_party_markup` | 10-15% | Markup applied to subcontracted/third-party positions |
