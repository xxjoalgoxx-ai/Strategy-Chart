import { useState, useRef, useEffect } from "react";

// ─── Synthetic price data ──────────────────────────────────────────────────
// We'll build a story: downtrend → undervalued zone → bounce entry → 
// trendline tap 1,2,3 → break entry → rally to target → trail stop
function generateCandles() {
  const candles = [];
  let price = 485;
  const seed = (n) => Math.sin(n * 127.1 + 311.7) * 0.5 + 0.5;

  const push = (o, h, l, c, tag) => candles.push({ o, h, l, c, tag: tag || null });

  // Phase A: Downtrend establishing (candles 0–10)
  // Lower highs and lower lows
  const downMoves = [
    [488,493,483,484],[484,487,479,480],[481,483,475,476],
    [477,480,471,473],[474,476,468,469],[470,473,464,466],
    [467,470,461,463],[464,467,458,460],[461,464,456,457],
    [458,461,453,455],[456,459,451,452]
  ];
  downMoves.forEach(([o,h,l,c], i) => push(o,h,l,c, i===0 ? "start" : null));

  // Phase B: Price hits undervalued zone, first bounce (candles 11–16)
  push(452,455,446,448); // touches zone bottom
  push(448,453,445,451); // another touch
  push(450,456,448,455, "zone_touch"); // reaction up — undervalued zone
  push(454,459,452,457);
  push(456,462,454,460);
  push(459,464,457,461);

  // Phase C: Pull back, tap 1 on downtrend trendline (candles 17–21)
  push(460,466,458,464, "tap1"); // TAP 1 on trendline
  push(463,468,461,466);
  push(465,470,462,467);
  push(466,471,464,469);
  push(468,472,461,463); // rejection back down

  // Phase D: Second dip, tap 2 on trendline (candles 22–27)
  push(462,465,458,460);
  push(460,463,455,457);
  push(456,460,452,458, "tap2"); // TAP 2 on trendline
  push(457,462,455,460);
  push(459,464,457,462);
  push(461,466,459,464);

  // Phase E: Another pullback, tap 3 — bounce entry (candles 28–34)
  push(463,467,460,461);
  push(461,464,457,459);
  push(458,462,454,460, "tap3_bounce"); // TAP 3 — BOUNCE ENTRY
  push(460,468,458,466, "bounce_entry"); // hammer / engulf candle — ENTER LONG
  push(465,471,463,469);
  push(468,474,466,472);
  push(471,477,469,475);

  // Phase F: Trendline break candle (candles 35–39)
  push(474,480,472,478);
  push(477,484,475,482, "break_candle"); // BREAK through downtrend trendline
  push(481,487,479,485, "break_confirm"); // confirmation candle above
  push(484,490,482,488);
  push(487,493,485,491);

  // Phase G: Rally toward target — trail stop shown (candles 40–47)
  push(490,497,488,495);
  push(494,501,492,499, "partial_exit"); // PARTIAL EXIT at first target
  push(498,505,496,503);
  push(502,508,499,506);
  push(505,511,503,509, "trail_stop_1"); // trail stop moves up
  push(508,514,505,512);
  push(511,517,508,515, "trail_stop_2");
  push(514,520,511,518, "final_target"); // FINAL TARGET HIT

  return candles;
}

const CANDLES = generateCandles();

// Key price levels
const UNDERVALUED_TOP = 460;
const UNDERVALUED_BOT = 446;
const OVERVALUED_TOP = 520;
const OVERVALUED_BOT = 508;
const TRENDLINE_START = { x: 0, y: 488 }; // approximate downtrend line
const TRENDLINE_END   = { x: 37, y: 475 }; // where it breaks

// Trendline points (candle index, price)
const TL_POINTS = [
  { ci: 0,  p: 493 },
  { ci: 17, p: 466 },
  { ci: 23, p: 463 },  // approx
  { ci: 30, p: 462 },
  { ci: 36, p: 484 },  // break point
];

const ANNOTATIONS = {
  tap1:         { color: "#C8A96E", label: "TAP 1",         side: "right", detail: "First trendline touch. Wick respects line. 1 of 3 needed." },
  tap2:         { color: "#C8A96E", label: "TAP 2",         side: "right", detail: "Second tap, 6+ candles after tap 1. Line still holding." },
  tap3_bounce:  { color: "#C8A96E", label: "TAP 3",         side: "left",  detail: "Third tap — trendline now has 3 quality touches. A+ criteria met." },
  bounce_entry: { color: "#7EB8A4", label: "BOUNCE ENTRY",  side: "left",  detail: "Hammer candle at trendline = Action Line triggered. Enter long. Stop = trendline close-through." },
  zone_touch:   { color: "#A78BC4", label: "UNDERVALUED ZONE", side: "left", detail: "Craig: price in undervalued area. RSI oversold. Demand > supply here. Bias = LONG." },
  break_candle: { color: "#E07B6A", label: "BREAK CANDLE",  side: "right", detail: "Full candle closes above downtrend trendline. Break entry triggered via market order." },
  break_confirm:{ color: "#7EB8A4", label: "BREAK ENTRY",   side: "right", detail: "Enter on break confirmation. Safety line = opposing trendline drawn below entry." },
  partial_exit: { color: "#B5C96E", label: "PARTIAL EXIT",  side: "right", detail: "First target hit (2R). Take 50% off. Trail stop on remaining position." },
  trail_stop_1: { color: "#6A9FD8", label: "TRAIL STOP ↑",  side: "right", detail: "Move stop up to below last swing low. Lock in profit as price makes new highs." },
  final_target: { color: "#B5C96E", label: "FINAL TARGET",  side: "right", detail: "Second target hit (Craig's next supply zone). Close remaining position. Full exit." },
};

const LEGEND = [
  { color: "#A78BC4", label: "Craig's Undervalued Zone", desc: "Where demand > supply. Bias = Long." },
  { color: "#E07B6A", label: "Craig's Overvalued Zone",  desc: "Where supply > demand. Bias = Short." },
  { color: "#C8A96E", label: "Tori's Trendline",         desc: "3 taps, 6+ candles apart, <45° slope." },
  { color: "#7EB8A4", label: "Entry (Bounce / Break)",    desc: "Action Line triggered. Enter here." },
  { color: "#B5C96E", label: "Target / Exit",             desc: "2R+ zone. Partial at first, full at second." },
  { color: "#6A9FD8", label: "Trailing Stop",             desc: "Moves up with trendline as price rises." },
];

export default function StrategyChart() {
  const svgRef = useRef(null);
  const [hovered, setHovered] = useState(null);
  const [activeAnnotation, setActiveAnnotation] = useState(null);
  const [activeTab, setActiveTab] = useState("bounce"); // bounce | break | zones

  const PAD = { top: 40, right: 24, bottom: 48, left: 52 };
  const W = 860;
  const H = 420;
  const chartW = W - PAD.left - PAD.right;
  const chartH = H - PAD.top - PAD.bottom;

  const allPrices = CANDLES.flatMap(c => [c.h, c.l]);
  const minP = Math.min(...allPrices) - 8;
  const maxP = Math.max(...allPrices) + 12;

  const cx = (i) => PAD.left + (i + 0.5) * (chartW / CANDLES.length);
  const cy = (p) => PAD.top + ((maxP - p) / (maxP - minP)) * chartH;
  const candleW = Math.max(2, (chartW / CANDLES.length) * 0.6);

  // RSI approximation (simplified, just for visual)
  const rsiData = CANDLES.map((c, i) => {
    // Simple momentum-based fake RSI
    if (i < 5) return 35 - i * 2;
    if (i < 12) return 25 + (i - 5) * 3;
    if (i < 20) return 42 + Math.sin(i) * 5;
    if (i < 30) return 38 + (i - 20) * 2;
    if (i < 38) return 55 + (i - 30) * 2.5;
    return Math.min(72, 75 - (i - 38) * 0.5 + Math.sin(i) * 2);
  });

  // Trendline coords
  const tlStartX = cx(0);
  const tlStartY = cy(493);
  const tlEndX = cx(36);
  const tlEndY = cy(478);

  // Safety line (after break — horizontal support)
  const safetyY = cy(474);

  // Filtered candles by tab
  const highlightRange = {
    bounce: [28, 36],
    break:  [34, 42],
    zones:  [10, 20],
  }[activeTab];

  const isHighlighted = (i) => i >= highlightRange[0] && i <= highlightRange[1];

  const tabAnnotations = {
    bounce: ["tap1","tap2","tap3_bounce","bounce_entry","zone_touch"],
    break:  ["tap3_bounce","break_candle","break_confirm","partial_exit","trail_stop_1","final_target"],
    zones:  ["zone_touch","tap1","tap2"],
  }[activeTab];

  return (
    <div style={{ background: "#0F1117", minHeight: "100vh", fontFamily: "'Inter','Segoe UI',sans-serif", color: "#E8E4DC", padding: "0 0 32px" }}>

      {/* Header */}
      <div style={{ background: "#161820", borderBottom: "1px solid #2A2D3A", padding: "14px 20px", display: "flex", alignItems: "center", justifyContent: "space-between" }}>
        <div>
          <div style={{ fontSize: 10, letterSpacing: "0.14em", color: "#C8A96E", textTransform: "uppercase", marginBottom: 2 }}>Visual Guide · Tori × Craig</div>
          <div style={{ fontSize: 17, fontWeight: 700 }}>Strategy Chart — Annotated Example</div>
        </div>
        <div style={{ fontSize: 11, color: "#5A5E6E" }}>Fictional asset · Educational only</div>
      </div>

      {/* Tab selector */}
      <div style={{ display: "flex", gap: 8, padding: "14px 20px 0" }}>
        {[
          { id: "bounce", label: "Bounce Entry",  color: "#7EB8A4" },
          { id: "break",  label: "Break Entry",   color: "#E07B6A" },
          { id: "zones",  label: "Craig's Zones", color: "#A78BC4" },
        ].map(tab => (
          <button key={tab.id} onClick={() => { setActiveTab(tab.id); setActiveAnnotation(null); }}
            style={{ padding: "7px 16px", borderRadius: 8, border: activeTab === tab.id ? `1.5px solid ${tab.color}` : "1.5px solid #2A2D3A",
              background: activeTab === tab.id ? `${tab.color}18` : "transparent",
              color: activeTab === tab.id ? tab.color : "#6B7080", cursor: "pointer", fontSize: 12, fontWeight: 600 }}>
            {tab.label}
          </button>
        ))}
        <div style={{ marginLeft: "auto", fontSize: 11, color: "#5A5E6E", alignSelf: "center" }}>
          Tap any annotation marker for details
        </div>
      </div>

      {/* Chart */}
      <div style={{ padding: "10px 20px 0", overflowX: "auto" }}>
        <svg ref={svgRef} width={W} height={H} style={{ display: "block", maxWidth: "100%" }}>
          <defs>
            <pattern id="grid" width={chartW / CANDLES.length * 4} height="20" patternUnits="userSpaceOnUse" x={PAD.left}>
              <line x1="0" y1="0" x2="0" y2={chartH + PAD.top} stroke="#1E2130" strokeWidth="0.5" />
            </pattern>
            <linearGradient id="uvGrad" x1="0" y1="0" x2="0" y2="1">
              <stop offset="0%" stopColor="#A78BC4" stopOpacity="0.18" />
              <stop offset="100%" stopColor="#A78BC4" stopOpacity="0.04" />
            </linearGradient>
            <linearGradient id="ovGrad" x1="0" y1="0" x2="0" y2="1">
              <stop offset="0%" stopColor="#E07B6A" stopOpacity="0.04" />
              <stop offset="100%" stopColor="#E07B6A" stopOpacity="0.18" />
            </linearGradient>
          </defs>

          {/* Background */}
          <rect width={W} height={H} fill="#0F1117" />
          <rect x={PAD.left} y={PAD.top} width={chartW} height={chartH} fill="#13151E" rx="4" />
          <rect x={PAD.left} y={PAD.top} width={chartW} height={chartH} fill="url(#grid)" />

          {/* Undervalued zone */}
          <rect x={PAD.left} y={cy(UNDERVALUED_TOP)} width={chartW}
            height={cy(UNDERVALUED_BOT) - cy(UNDERVALUED_TOP)} fill="url(#uvGrad)" />
          <line x1={PAD.left} y1={cy(UNDERVALUED_TOP)} x2={PAD.left + chartW} y2={cy(UNDERVALUED_TOP)}
            stroke="#A78BC4" strokeWidth="0.8" strokeDasharray="5,4" opacity="0.6" />
          <line x1={PAD.left} y1={cy(UNDERVALUED_BOT)} x2={PAD.left + chartW} y2={cy(UNDERVALUED_BOT)}
            stroke="#A78BC4" strokeWidth="0.8" strokeDasharray="5,4" opacity="0.6" />
          <text x={PAD.left + 5} y={cy(UNDERVALUED_TOP) - 4} fill="#A78BC4" fontSize="9" opacity="0.8">UNDERVALUED ZONE (Craig)</text>

          {/* Overvalued zone */}
          <rect x={PAD.left} y={cy(OVERVALUED_TOP)} width={chartW}
            height={cy(OVERVALUED_BOT) - cy(OVERVALUED_TOP)} fill="url(#ovGrad)" />
          <line x1={PAD.left} y1={cy(OVERVALUED_TOP)} x2={PAD.left + chartW} y2={cy(OVERVALUED_TOP)}
            stroke="#E07B6A" strokeWidth="0.8" strokeDasharray="5,4" opacity="0.6" />
          <line x1={PAD.left} y1={cy(OVERVALUED_BOT)} x2={PAD.left + chartW} y2={cy(OVERVALUED_BOT)}
            stroke="#E07B6A" strokeWidth="0.8" strokeDasharray="5,4" opacity="0.6" />
          <text x={PAD.left + 5} y={cy(OVERVALUED_TOP) - 4} fill="#E07B6A" fontSize="9" opacity="0.8">OVERVALUED ZONE (Craig)</text>

          {/* Downtrend trendline (Tori) */}
          <line x1={tlStartX} y1={tlStartY} x2={tlEndX} y2={tlEndY}
            stroke="#C8A96E" strokeWidth="1.5" strokeDasharray="6,3" opacity="0.9" />
          <text x={tlStartX + 4} y={tlStartY - 6} fill="#C8A96E" fontSize="9" fontWeight="600">Tori's Trendline (downtrend)</text>

          {/* Safety line after break */}
          {(activeTab === "break") && (
            <>
              <line x1={cx(36)} y1={safetyY} x2={cx(47)} y2={safetyY}
                stroke="#6A9FD8" strokeWidth="1" strokeDasharray="4,3" opacity="0.8" />
              <text x={cx(37)} y={safetyY - 5} fill="#6A9FD8" fontSize="8.5">Safety Line (stop)</text>
            </>
          )}

          {/* Trailing stop steps */}
          {(activeTab === "break") && (
            <>
              <line x1={cx(40)} y1={cy(482)} x2={cx(44)} y2={cy(482)}
                stroke="#6A9FD8" strokeWidth="1.2" strokeDasharray="3,2" opacity="0.7" />
              <line x1={cx(44)} y1={cy(495)} x2={cx(47)} y2={cy(495)}
                stroke="#6A9FD8" strokeWidth="1.2" strokeDasharray="3,2" opacity="0.7" />
              <line x1={cx(44)} y1={cy(482)} x2={cx(44)} y2={cy(495)}
                stroke="#6A9FD8" strokeWidth="0.8" opacity="0.5" />
              <text x={cx(40) + 2} y={cy(482) - 4} fill="#6A9FD8" fontSize="7.5">Trail ↑</text>
            </>
          )}

          {/* Price grid lines */}
          {[450, 460, 470, 480, 490, 500, 510, 520].map(p => (
            <g key={p}>
              <line x1={PAD.left} y1={cy(p)} x2={PAD.left + chartW} y2={cy(p)}
                stroke="#1E2130" strokeWidth="0.5" />
              <text x={PAD.left - 4} y={cy(p) + 3.5} fill="#5A5E6E" fontSize="9" textAnchor="end">{p}</text>
            </g>
          ))}

          {/* Candles */}
          {CANDLES.map((c, i) => {
            const x = cx(i);
            const isUp = c.c >= c.o;
            const bodyTop = cy(Math.max(c.o, c.c));
            const bodyBot = cy(Math.min(c.o, c.c));
            const bodyH = Math.max(1.5, bodyBot - bodyTop);
            const dimmed = !isHighlighted(i) && activeTab !== "zones";
            const annotKey = c.tag;
            const hasAnnot = annotKey && tabAnnotations.includes(annotKey);
            const baseColor = isUp ? "#7EB8A4" : "#E07B6A";
            const opacity = dimmed ? 0.25 : 1;

            return (
              <g key={i} opacity={opacity}
                style={{ cursor: hasAnnot ? "pointer" : "default" }}
                onClick={() => hasAnnot && setActiveAnnotation(activeAnnotation === annotKey ? null : annotKey)}>
                {/* Wick */}
                <line x1={x} y1={cy(c.h)} x2={x} y2={cy(c.l)} stroke={baseColor} strokeWidth="1" />
                {/* Body */}
                <rect x={x - candleW / 2} y={bodyTop} width={candleW} height={bodyH}
                  fill={isUp ? baseColor : baseColor} stroke={baseColor} strokeWidth="0.5"
                  fillOpacity={isUp ? 1 : 0.2} />
                {/* Annotation dot */}
                {hasAnnot && (
                  <circle cx={x} cy={isUp ? cy(c.h) - 10 : cy(c.l) + 10} r="5"
                    fill={ANNOTATIONS[annotKey].color}
                    stroke="#0F1117" strokeWidth="1.5"
                    opacity={activeAnnotation === annotKey ? 1 : 0.85} />
                )}
              </g>
            );
          })}

          {/* Tap labels on trendline */}
          {[17, 28].map((ci, idx) => (
            <text key={ci} x={cx(ci)} y={tlStartY + (tlEndY - tlStartY) * (ci / 36) - 12}
              fill="#C8A96E" fontSize="8" textAnchor="middle" fontWeight="600" opacity="0.9">
              Tap {idx + 2}
            </text>
          ))}
          <text x={cx(0)} y={tlStartY - 2} fill="#C8A96E" fontSize="8" textAnchor="middle" fontWeight="600" opacity="0.9">Tap 1</text>

          {/* X axis labels */}
          {[0, 10, 20, 30, 40, 47].map(i => (
            <text key={i} x={cx(Math.min(i, CANDLES.length - 1))} y={H - PAD.bottom + 14}
              fill="#5A5E6E" fontSize="8.5" textAnchor="middle">
              {["Week 1","Week 3","Week 5","Week 7","Week 9","Week 11"][Math.floor(i / 8)]}
            </text>
          ))}

          {/* Active annotation callout */}
          {activeAnnotation && (() => {
            const ann = ANNOTATIONS[activeAnnotation];
            const ci = CANDLES.findIndex(c => c.tag === activeAnnotation);
            const x = cx(ci);
            const c = CANDLES[ci];
            const isUp = c.c >= c.o;
            const anchorY = isUp ? cy(c.h) - 16 : cy(c.l) + 16;
            const boxW = 190;
            const boxH = 44;
            const onRight = ann.side === "right";
            const boxX = onRight ? Math.min(x + 10, W - boxW - 10) : Math.max(x - boxW - 10, PAD.left);
            const boxY = Math.max(PAD.top + 2, Math.min(anchorY - boxH / 2, H - PAD.bottom - boxH - 4));

            return (
              <g>
                <line x1={x} y1={anchorY} x2={onRight ? boxX : boxX + boxW} y2={boxY + boxH / 2}
                  stroke={ann.color} strokeWidth="1" strokeDasharray="3,2" opacity="0.7" />
                <rect x={boxX} y={boxY} width={boxW} height={boxH} rx="5"
                  fill="#161820" stroke={ann.color} strokeWidth="1.2" />
                <text x={boxX + 8} y={boxY + 14} fill={ann.color} fontSize="9" fontWeight="700">{ann.label}</text>
                {/* Word wrap detail */}
                {(() => {
                  const words = ann.detail.split(" ");
                  const lines = [];
                  let line = "";
                  const maxW = boxW - 16;
                  // approx char width at 8px ~ 4.5px
                  words.forEach(w => {
                    const test = line + (line ? " " : "") + w;
                    if (test.length * 4.2 > maxW) { lines.push(line); line = w; }
                    else line = test;
                  });
                  if (line) lines.push(line);
                  return lines.slice(0, 2).map((ln, li) => (
                    <text key={li} x={boxX + 8} y={boxY + 26 + li * 11} fill="#A0A4B0" fontSize="8">{ln}</text>
                  ));
                })()}
              </g>
            );
          })()}
        </svg>
      </div>

      {/* RSI strip */}
      <div style={{ padding: "0 20px" }}>
        <svg width={W} height={70} style={{ display: "block", maxWidth: "100%" }}>
          <rect width={W} height={70} fill="#0F1117" />
          <rect x={PAD.left} y={5} width={chartW} height={55} fill="#13151E" rx="3" />
          <text x={PAD.left - 4} y={12} fill="#5A5E6E" fontSize="8" textAnchor="end">RSI</text>
          {/* Overbought/oversold lines */}
          <line x1={PAD.left} y1={5 + 55 * (1 - 70/100)} x2={PAD.left + chartW} y2={5 + 55 * (1 - 70/100)}
            stroke="#E07B6A" strokeWidth="0.6" strokeDasharray="3,3" opacity="0.6" />
          <line x1={PAD.left} y1={5 + 55 * (1 - 30/100)} x2={PAD.left + chartW} y2={5 + 55 * (1 - 30/100)}
            stroke="#A78BC4" strokeWidth="0.6" strokeDasharray="3,3" opacity="0.6" />
          <text x={PAD.left + 3} y={5 + 55 * (1 - 70/100) - 2} fill="#E07B6A" fontSize="7" opacity="0.8">70</text>
          <text x={PAD.left + 3} y={5 + 55 * (1 - 30/100) - 2} fill="#A78BC4" fontSize="7" opacity="0.8">30</text>
          {/* RSI line */}
          <polyline
            points={rsiData.map((r, i) => `${cx(i)},${5 + 55 * (1 - r / 100)}`).join(" ")}
            fill="none" stroke="#C8A96E" strokeWidth="1.4" opacity="0.9" />
          {/* Oversold highlight */}
          <text x={cx(8)} y={62} fill="#A78BC4" fontSize="7.5" textAnchor="middle">Oversold (Craig confirm)</text>
          <text x={cx(40)} y={62} fill="#E07B6A" fontSize="7.5" textAnchor="middle">Overbought</text>
        </svg>
      </div>

      {/* Legend & Annotation list */}
      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 12, padding: "10px 20px 0" }}>
        {/* Legend */}
        <div style={{ background: "#161820", borderRadius: 10, border: "1px solid #2A2D3A", padding: "14px 16px" }}>
          <div style={{ fontSize: 10, letterSpacing: "0.12em", color: "#C8A96E", textTransform: "uppercase", marginBottom: 10 }}>Legend</div>
          {LEGEND.map(l => (
            <div key={l.label} style={{ display: "flex", alignItems: "flex-start", gap: 8, marginBottom: 8 }}>
              <div style={{ width: 10, height: 10, borderRadius: 2, background: l.color, flexShrink: 0, marginTop: 2 }} />
              <div>
                <div style={{ fontSize: 11, fontWeight: 600, color: "#E8E4DC" }}>{l.label}</div>
                <div style={{ fontSize: 10, color: "#6B7080" }}>{l.desc}</div>
              </div>
            </div>
          ))}
        </div>

        {/* Annotation guide for active tab */}
        <div style={{ background: "#161820", borderRadius: 10, border: "1px solid #2A2D3A", padding: "14px 16px" }}>
          <div style={{ fontSize: 10, letterSpacing: "0.12em", color: "#C8A96E", textTransform: "uppercase", marginBottom: 10 }}>
            {activeTab === "bounce" ? "Bounce Entry Walkthrough" : activeTab === "break" ? "Break Entry Walkthrough" : "Craig's Zones Walkthrough"}
          </div>
          {tabAnnotations.map((key, i) => {
            const ann = ANNOTATIONS[key];
            if (!ann) return null;
            return (
              <div key={key}
                onClick={() => setActiveAnnotation(activeAnnotation === key ? null : key)}
                style={{ display: "flex", alignItems: "flex-start", gap: 8, marginBottom: 8, cursor: "pointer",
                  background: activeAnnotation === key ? `${ann.color}15` : "transparent",
                  borderRadius: 6, padding: "4px 6px", margin: "0 -6px 4px", border: activeAnnotation === key ? `1px solid ${ann.color}40` : "1px solid transparent" }}>
                <div style={{ width: 18, height: 18, borderRadius: "50%", background: ann.color, flexShrink: 0,
                  display: "flex", alignItems: "center", justifyContent: "center", fontSize: 9, fontWeight: 700, color: "#0F1117" }}>
                  {i + 1}
                </div>
                <div>
                  <div style={{ fontSize: 11, fontWeight: 600, color: ann.color }}>{ann.label}</div>
                  <div style={{ fontSize: 10, color: "#6B7080", lineHeight: 1.4 }}>{ann.detail}</div>
                </div>
              </div>
            );
          })}
        </div>
      </div>

      {/* Bottom tip */}
      <div style={{ margin: "12px 20px 0", background: "#161820", borderRadius: 8, border: "1px solid #2A2D3A", padding: "10px 14px", fontSize: 11, color: "#6B7080", display: "flex", gap: 8 }}>
        <span style={{ color: "#C8A96E" }}>💡</span>
        <span>This is a fictional educational chart. Real setups take days to weeks to form. The key: Craig's zone tells you <strong style={{ color: "#A78BC4" }}>where</strong> to look — Tori's trendline tells you <strong style={{ color: "#7EB8A4" }}>when</strong> to act.</span>
      </div>
    </div>
  );
}
