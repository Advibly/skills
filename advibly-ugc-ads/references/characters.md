# Creator Character Archetypes

Eight pre-built character image prompts tuned for realism with gpt-image-2. Use them as the `prompt` for `advibly_generate_image` as-is, or modify the outfit/environment to match the brand context.

**Generation settings for every archetype:**

```
advibly_generate_image
  model: "gpt-image-2"
  aspect_ratio: "2:3"
  quality: "high"
```

Do not pass `brand_id` (or set `on_brand: false`). A creator reference must look like a real person's phone photo; brand-kit references pull it toward branded collateral. The finished image URL becomes the creator reference for every video shot.

---

## Archetype 1 - Fitness Woman (25-30)

Best for: supplements, activewear, protein, wellness products, health-adjacent DTC

```
Candid iPhone photo of a 27-year-old woman, medium-length auburn hair with natural flyaways, light freckles across nose and cheeks, minimal makeup, just mascara, wearing a fitted lavender athletic tank top and matching leggings. Standing in a bright kitchen, slight low angle as if she's holding the phone herself. Soft morning window light from the left, slightly uneven exposure on the right side of her face. Real, unretouched skin texture with visible pores. Natural hair loose strands around face. No studio lighting. Editorial-style realism.
```

---

## Archetype 2 - Fitness Man (25-32)

Best for: supplements, protein, pre-workout, gym gear, men's wellness

```
Candid iPhone photo of a 28-year-old man, medium-length dark brown hair slightly damp, stubble 2-3 days, no makeup, wearing a fitted grey athletic t-shirt, slight sweat at collar. Standing in a gym with equipment visible and slightly blurred in background. Shot from slightly below eye level as if he's holding the phone. Overhead gym fluorescent light, slightly harsh on shoulders, softer on face. Real skin texture, visible pores, slight post-workout flush. Natural imperfect framing, slightly off-center. No studio lighting.
```

---

## Archetype 3 - Busy Mom (30-38)

Best for: home goods, organization, family wellness, convenience food/beverage, kids products, time-saving products

```
Candid iPhone photo of a 34-year-old woman, shoulder-length dark brown hair in a casual half-up, no makeup except light lip balm, small gold stud earrings, wearing a relaxed linen button-down over a white tank top. Standing in a kitchen with dishes and a coffee mug visible on the counter behind her. Shot at eye level, slightly off-center. Warm morning kitchen overhead light, slightly softer from a window to the right. Real skin texture, slight under-eye tiredness. Natural flyaways. No studio lighting. Candid, unposed energy.
```

---

## Archetype 4 - College Student (20-24)

Best for: energy drinks, snacks, student productivity tools, fashion, affordable DTC, anything trending on TikTok

```
Candid iPhone photo of a 22-year-old woman, long blonde hair with natural wave and flyaways, no makeup, small nose ring, wearing an oversized vintage graphic crewneck and high-waist jeans. Sitting in a car driver's seat, residential neighborhood visible through window behind her. Shot slightly above eye level as if holding phone up. Soft overcast afternoon light through windshield, slightly cool. Real skin texture, light freckles across bridge of nose. Hair tousled, not styled. No studio lighting. Candid, slightly bored energy.
```

---

## Archetype 5 - Professional Woman (28-36)

Best for: beauty, skincare, professional wellness, productivity tools, premium DTC

```
Candid iPhone photo of a 31-year-old woman, dark black hair pulled back loosely with strands framing face, natural makeup, light foundation, neutral lip, brow gel only, wearing a soft cream fitted crewneck. Standing in a home office or bright apartment living space, bookshelf slightly blurred in background. Shot at eye level, slightly off-center. Soft natural window light from the right. Real skin texture. Hair slightly imperfect, not salon-fresh. No studio lighting. Calm, composed energy.
```

---

## Archetype 6 - Outdoors Guy (27-35)

Best for: outdoor gear, supplements, protein, adventure/travel products, workwear-adjacent DTC

```
Candid iPhone photo of a 30-year-old man, medium brown hair slightly wind-tousled, 3-day stubble, wearing a worn olive green quarter-zip fleece over a white t-shirt. Standing outdoors with trees or hills visible and slightly blurred behind him. Shot at eye level, slightly off-center. Overcast natural outdoor light, soft and even, no shadows. Real skin texture, slight sun-weathered quality. Hair disheveled from wind. No studio lighting. Relaxed, outdoorsy energy.
```

---

## Archetype 7 - Beauty Creator (22-29)

Best for: makeup, skincare, haircare, fragrance, beauty accessories, anything in the beauty/personal care space

```
Candid iPhone photo of a 25-year-old woman, long wavy dark brunette hair, full but natural makeup, skin-toned foundation, soft blush, mascara, glossy lip, wearing a fitted white ribbed tank top. Standing in a bright bathroom with a white tile backsplash and a pothos plant on the shelf behind her, slightly blurred. Shot slightly above eye level as if holding phone in front of vanity mirror. Natural vanity light, warm, slightly overhead. Real skin texture, makeup is visible as makeup, not airbrushed. Slight hair flyaways. No studio lighting. Warm, friendly energy.
```

---

## Archetype 8 - Everyday Guy (24-34)

Best for: household products, food/beverage, consumer tech, anything with a "regular person" trust signal; most DTC categories benefit from this archetype

```
Candid iPhone photo of a 29-year-old man, short dark hair, clean-shaven, no distinctive features, wearing a plain navy blue crewneck sweatshirt. Sitting on a couch in a living room with a TV remote and a water bottle on the coffee table slightly blurred behind him. Shot at eye level, slightly off-center. Warm overhead lamp light from the left, slightly softer ambient from a window to the right. Real skin texture. No studio lighting. Neutral, relaxed, completely unperformative energy, looks like someone who picked up their phone to make a quick video.
```

---

## How to customize these

Every archetype is a starting point. Modify:

- **Outfit** - match to brand aesthetic. Athletic / casual / professional / fashion
- **Environment** - match to product context. Kitchen / car / gym / bathroom / living room / outdoors
- **Age range** - adjust to match the brand's audience (the ICP section of the brand brief from `advibly_get_brand` is the source here)
- **Hair/features** - adjust for demographic diversity

Always keep:
- "Candid iPhone photo" in the prompt
- Specific imperfections (flyaways, freckles, slight imperfect lighting)
- A real environment in the frame even for the character reference image
- "No studio lighting"
- "Real skin texture" or "Real, unretouched skin texture"

These are the signals that separate a UGC creator image from a stock photo or AI model image.
