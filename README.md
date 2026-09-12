Indian Road Sign Detection API 🚦

Detect and classify 78 types of Indian road signs from dashcam imagery using a two-stage deep learning pipeline built on the IRC:67-2012 standard.

Python 
ONNX 
mAP 
Classes 
License

Try the API → RapidAPI Marketplace

Live Demo → skotantrx.com/demo.html
⸻
The Problem

India follows the IRC (Indian Roads Congress) sign standard — over 100 sign types across regulatory, warning, and informatory categories. But real-world conditions make detection hard:

- Faded paint, dust, occlusion
- Night glare, rain, fog
- Non-standard placement and sizing
- State-specific variations not in the IRC standard

Most ADAS systems and sign recognition models are trained on European (GTSRB) or US sign datasets. This API is built specifically for Indian roads.
⸻
How It Works

Dashcam Frame → AI Locator → Crop → AI Classifier → Sign Type + Confidence
                  (find)             (identify)
                mAP50: 96.5%        78 IRC classes


Two-stage pipeline:
1. Stage 1 — Locator: AI model finds sign bounding boxes in the full frame (mAP50 = 96.5%)
2. Stage 2 — Classifier: Second AI model identifies the sign type from the cropped region (78 classes)
3. OCR (for speed limits): Reads the actual number on speed limit signs

Inference time: ~0.2 seconds per image on GPU
⸻
Supported Sign Classes (78)

Mandatory (7)
compulsory_keep_left · compulsory_keep_right · compulsory_sound_horn · compulsory_u_turn · compulsory_cycle_track · pass_either_side · roundabout

Prohibitory (14)
no_entry · no_parking · no_stopping · no_stopping_and_no_standing · one_way · overtaking_prohibited · u_turn_prohibited · left_turn_prohibited · right_turn_prohibited · heavy_vehicles_prohibited · two_wheelers_prohibited · pedestrian_prohibited · no_honking · no_littering

Warning (34)
go_slow · accident_prone_area_go_slow · speed_breaker_ahead · school_ahead · pedestrian_crossing · cross_road · t_intersection · y_intersection · side_road_left · side_road_right · left_hand_curve · right_hand_curve · left_hairpin_bend · right_hairpin_bend · left_reverse_bend · right_reverse_bend · series_of_bends · road_narrows_ahead · narrow_bridge_ahead · road_widens · steep_ascent · steep_descent · falling_rocks · men_at_work · gap_in_median · guarded_rail_crossing · merging_traffic_ahead_from_left · merging_traffic_ahead_from_right · overhead_electric_cables · reduced_carriageway_left_lane_is_reduced · rumble_strip_ahead · major_road_ahead · t_intersection_major_road_ahead · u_turn_ahead

Speed Limit (9)
maximum_speed_limit_20 · maximum_speed_limit_30 · maximum_speed_limit_35 · maximum_speed_limit_40 · maximum_speed_limit_50 · maximum_speed_limit_60 · maximum_speed_limit_80 · maximum_speed_limit_100 · maximum_speed_limit_120

Hazard Markers (8)
chevron_board_left · chevron_board_right · object_hazard_left · object_hazard_right · double_chevron_hazard_marker · triple_chevron_left · triple_chevron_right · two_way_hazard_marker

Informatory (5)
free_left · give_way · stop · height_limit · restriction_ends
⸻
Quick Start

Using the API (via RapidAPI)

curl -X POST "https://indian-road-sign-detection.p.rapidapi.com/api/v1/detect" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: indian-road-sign-detection.p.rapidapi.com" \
  -F "image=@dashcam_frame.jpg"


Python Example

import requests

url = "https://indian-road-sign-detection.p.rapidapi.com/api/v1/detect"
headers = {
    "X-RapidAPI-Key": "YOUR_API_KEY",
    "X-RapidAPI-Host": "indian-road-sign-detection.p.rapidapi.com",
}

with open("dashcam_frame.jpg", "rb") as f:
    response = requests.post(url, headers=headers, files={"image": f})

result = response.json()
for sign in result["detections"]:
    print(f"{sign['sign_type']} — {sign['confidence']:.0%} at [{sign['bbox']}]")


Response Format

{
  "detections": [
    {
      "sign_type": "maximum_speed_limit_40",
      "display_name": "Maximum Speed Limit 40",
      "category": "speed_limit",
      "confidence": 0.98,
      "bbox": [412, 85, 498, 192],
      "ocr_value": "40"
    },
    {
      "sign_type": "no_stopping_and_no_standing",
      "display_name": "No Stopping And No Standing",
      "category": "prohibitory",
      "confidence": 0.95,
      "bbox": [650, 110, 720, 205]
    }
  ],
  "inference_time_ms": 187,
  "image_size": [1920, 1080]
}

⸻
API Endpoints
Endpoint	Method	Description
/api/v1/detect	POST	Detect signs in an image
/api/v1/classes	GET	List all 78 supported sign classes
/api/v1/health	GET	API health check
/api/v1/report	POST	Report a missed or wrong detection
⸻
Use Cases

- ADAS / autonomous driving — speed limit enforcement, sign-aware navigation
- Fleet management — automated compliance monitoring across routes
- Insurance telematics — risk scoring from dashcam footage
- Road infrastructure mapping — sign inventory from street-view imagery
- Driving assessment — automated driving test evaluation (see Pālan)
⸻
Pricing
Tier	Calls/month	Price
Basic	50	Free
Pro	500	$10/mo
Ultra	5,000	$50/mo
Enterprise	Custom	Contact us

Subscribe on RapidAPI →
⸻
Training Data

- Built from real Indian dashcam footage (Hyderabad, Telangana)
- Annotated against the IRC:67-2012 standard (Indian Roads Congress)
- Covers highway, urban, suburban, and rural road conditions
- Day, evening, and night captures
- Multiple weather conditions (dry, rain, fog)
⸻
Built By

SKO TantrX — Break the Old. Build the Bold.

Deep tech company building computer vision and AI solutions for Indian roads.

- 🌐 skotantrx.com
- 💼 LinkedIn
- 📧 info@skotantrx.com
⸻
License

This API is a commercial product. The model weights and training data are proprietary.

The code samples and documentation in this repository are provided under the MIT License for reference purposes.
