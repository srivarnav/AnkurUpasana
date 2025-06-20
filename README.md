# Ankur Upasana
```
import React from "react";
import IconTooltip from "../components/common/IconTooltip";
import {
  ALT_INFO_ICON_MESSAGE,
  EXPENSE_RATIO_ERROR_MESSAGE,
  SMA_FEE_PCT_ERROR_MESSAGE,
} from "../constants/messages";
import PortfolioSummaryConstants from "../constants/PortfolioSummaryConstants";
import {
  isAllocationWithNullFeePresent,
  productListSelectors,
} from "../utils/helpers";

const renderLine = (key, value, display, errorTooltip, bottomId, testId = 'portfolio-summary-content-line') => {
  const isBottom = display === "Total";
  const className = isBottom ? "portfolio-summary-content-bottom-line" : "portfolio-summary-content-line";
  return (
    <div className={className} data-testid={testId} id={isBottom ? bottomId : undefined}>
      <p className={errorTooltip ? "content-line-with-warning-icon" : undefined}>
        {errorTooltip}
        {key}
      </p>
      <p>{value}</p>
    </div>
  );
};

const PortfolioSummaryContentSection = ({
  source,
  fieldName,
  line1Key,
  line1Value,
  line2Key,
  line2Value,
  line3Key,
  line3Value,
  line3Display,
  line4Key,
  line4Value,
  line4Display,
  line5Key,
  line5Value,
  line5Display,
}) => {
  const formattedFieldName = fieldName.toLowerCase().replace(/ /g, "_");
  const sectionId = `portfolio-summary-content-${formattedFieldName}`;
  const bottomLineId = `portfolio-summary-content-bottom-line-${formattedFieldName}`;

  const showAltInfoIcon =
    productListSelectors.hasAlternativeInvestmentProducts(source) &&
    fieldName === PortfolioSummaryConstants.EXPENSE_RATIO;

  const showExpenseRatioError =
    isAllocationWithNullFeePresent(source, "expenseRatio") &&
    fieldName === PortfolioSummaryConstants.EXPENSE_RATIO;

  const showSmaFeeError =
    isAllocationWithNullFeePresent(source, "smaFeePct") &&
    fieldName === PortfolioSummaryConstants.SMA_MANAGER_FEES;

  const showLine1AltInfoIcon =
    productListSelectors.hasAlternativeInvestmentProducts(source) &&
    line1Key === PortfolioSummaryConstants.EXPENSE_RATIO;

  const showLine2SmaFeeError =
    isAllocationWithNullFeePresent(source, "smaFeePct") &&
    line2Key === PortfolioSummaryConstants.SMA_MANAGER_FEES;

  return (
    <div className="portfolio-summary-content" data-testid="portfolio-summary-content" id={sectionId}>
      <div className="portfolio-summary-content-fieldname" data-testid="portfolio-summary-content-fieldname">
        <span className="portfolio-summary-content-fieldname-value">{fieldName}</span>

        {showAltInfoIcon && (
          <IconTooltip title="" placement="above" align="right" classes="alt-info-icon">
            <p>{ALT_INFO_ICON_MESSAGE}</p>
          </IconTooltip>
        )}

        {showExpenseRatioError && (
          <IconTooltip type="notice-warning" name="expense-ratio-error">
            <p>{EXPENSE_RATIO_ERROR_MESSAGE}</p>
          </IconTooltip>
        )}

        {showSmaFeeError && (
          <IconTooltip type="notice-warning" name="sma-fee-pct-error">
            <p>{SMA_FEE_PCT_ERROR_MESSAGE}</p>
          </IconTooltip>
        )}
      </div>

      {/* Line 1 */}
      <div className="portfolio-summary-content-line" data-testid="portfolio-summary-content-line">
        <p className="content-line-with-info-icon">
          {line1Key}
          {showLine1AltInfoIcon && (
            <IconTooltip title="" placement="above" align="right" classes="alt-info-icon">
              <p>{ALT_INFO_ICON_MESSAGE}</p>
            </IconTooltip>
          )}
        </p>
        <p>{line1Value}</p>
      </div>

      {/* Line 2 */}
      {line2Key && line2Value &&
        renderLine(
          line2Key,
          line2Value,
          "Line2",
          showLine2SmaFeeError && (
            <IconTooltip type="notice-warning" name="sma-fee-pct-error">
              <p>{SMA_FEE_PCT_ERROR_MESSAGE}</p>
            </IconTooltip>
          )
        )}

      {/* Line 3 */}
      {line3Key && line3Value && renderLine(line3Key, line3Value, line3Display, null, bottomLineId)}

      {/* Line 4 */}
      {line4Key && line4Value && renderLine(line4Key, line4Value, line4Display, null, bottomLineId)}

      {/* Line 5 */}
      {line5Key && line5Value && renderLine(line5Key, line5Value, line5Display, null, bottomLineId)}
    </div>
  );
};

export default PortfolioSummaryContentSection;
```
