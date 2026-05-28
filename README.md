<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Team Performance Regression Simulator</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: #f0f4f8;
            color: #333;
            padding: 30px;
            display: flex;
            justify-content: center;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.08);
            max-width: 650px;
            width: 100%;
        }
        h2 { margin-top: 0; color: #1e293b; border-bottom: 2px solid #f1f5f9; padding-bottom: 12px; }
        p { font-size: 14px; color: #64748b; line-height: 1.5; }
        
        .variable-container {
            margin-bottom: 24px;
        }
        
        .label-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
            font-weight: 600;
            font-size: 15px;
        }
        
        /* Variables Styling */
        #pc-group .var-name { color: #1d4ed8; } 
        #tc-group .var-name { color: #047857; } 
        #px-group .var-name { color: #b91c1c; } 
        #sd-group .var-name { color: #0f766e; }
        #error-group .var-name { color: #475569; font-weight: bold; }

        .current-badge {
            background: #f1f5f9;
            padding: 2px 10px;
            border-radius: 20px;
            font-size: 14px;
            border: 1px solid #e2e8f0;
        }

        /* Slider Bar Base Styling */
        .slider {
            -webkit-appearance: none;
            width: 100%;
            height: 10px;
            border-radius: 5px;
            outline: none;
            transition: opacity .2s;
        }
        
        /* Slider Track and Thumb Colors */
        #pc-slider { background: #93c5fd; }
        #pc-slider::-webkit-slider-thumb { background: #1d4ed8; }
        #pc-slider::-moz-range-thumb { background: #1d4ed8; }
        
        #tc-slider { background: #6ee7b7; }
        #tc-slider::-webkit-slider-thumb { background: #047857; }
        #tc-slider::-moz-range-thumb { background: #047857; }
        
        #px-slider { background: #fca5a5; }
        #px-slider::-webkit-slider-thumb { background: #b91c1c; }
        #px-slider::-moz-range-thumb { background: #b91c1c; }
        
        #sd-slider { background: #99f6e4; }
        #sd-slider::-webkit-slider-thumb { background: #0f766e; }
        #sd-slider::-moz-range-thumb { background: #0f766e; }

        #error-slider { background: #cbd5e1; }
        #error-slider::-webkit-slider-thumb { background: #475569; }
        #error-slider::-moz-range-thumb { background: #475569; }

        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
        }
        .slider::-moz-range-thumb {
            width: 22px;
            height: 22px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
        }

        .ticks {
            display: flex;
            justify-content: space-between;
            padding: 0 5px;
            margin-top: 6px;
            color: #64748b; 
            font-size: 12px;
            font-weight: 600;
        }

        /* Error Output Display */
        .error-display-box {
            background: #f8fafc;
            border: 1px dashed #cbd5e1;
            padding: 12px;
            border-radius: 8px;
            text-align: center;
            margin-top: 20px;
            font-size: 14px;
            font-weight: 600;
            color: #475569;
        }

        /* Result Dashboard */
        .result-box {
            background: #1e293b;
            color: white;
            padding: 22px;
            border-radius: 12px;
            text-align: center;
            margin-top: 15px;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
        }
        .result-val { font-size: 3.5rem; font-weight: 800; color: #38bdf8; margin: 5px 0; }
        
        .rerun-btn {
            background: #334155;
            color: #f8fafc;
            border: 1px solid #475569;
            padding: 6px 12px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 12px;
            font-weight: 600;
            margin-top: 8px;
            transition: background 0.2s;
        }
        .rerun-btn:hover { background: #475569; }

        .formula-box {
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            font-family: 'Courier New', Courier, monospace;
            padding: 14px;
            border-radius: 8px;
            font-size: 13px;
            margin-top: 20px;
            color: #475569;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Team Performance (TP) Simulator</h2>
    <p>Drag the sliders to adjust your parameters. The model now calculates with a Gaussian error term to simulate real-world noise.</p>
    
    <div class="variable-container" id="pc-group">
        <div class="label-row">
            <span class="var-name">Psychological Climate (PC)</span>
            <span class="current-badge" id="pc-val-label">Value: 5</span>
        </div>
        <input type="range" min="1" max="10" value="5" class="slider" id="pc-slider" oninput="calculateModel(false)">
        <div class="ticks"><span>1</span><span>2</span><span>3</span><span>4</span><span>5</span><span>6</span><span>7</span><span>8</span><span>9</span><span>10</span></div>
    </div>

    <div class="variable-container" id="tc-group">
        <div class="label-row">
            <span class="var-name">Team Composition (TC)</span>
            <span class="current-badge" id="tc-val-label">
