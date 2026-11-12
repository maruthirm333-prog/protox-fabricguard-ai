# Roadmap

## Phase 1: Hardware (partially completed)

- [x] OLED wiring and display test
- [x] PSRAM detection
- [x] Camera initialisation
- [ ] Fix FB-OVF (frame-buffer overflow)
- [ ] Stable image capture

## Phase 2: AI development (needs improvement)

- [x] AITEX dataset preparation
- [x] Initial CNN training (binary)
- [ ] Genuine three-class dataset
- [ ] Train the three-class model
- [ ] Test on unseen real fabric

## Phase 3: Integration (pending)

- [ ] Laptop server
- [ ] ESP32-to-laptop communication
- [ ] AI prediction response
- [ ] OLED result display
- [ ] Live dashboard testing

## Phase 4: Final product (pending)

- [ ] Fabric inspection arrangement
- [ ] Consistent lighting
- [ ] Enclosure and mounting
- [ ] Continuous inspection testing
- [ ] Windows application packaging
- [ ] Client demonstration and documentation

## Phase 5: GitHub

- [x] Create README
- [x] Publish repository (documentation first)
- [ ] Organise complete source code
- [ ] Add circuit diagrams
- [ ] Add installation instructions
- [ ] Add sample images

## Final product requirements

1. Automatically capture fabric images
2. Identify normal and defective fabric
3. Distinguish broken and scratch defects
4. Display the result on the OLED
5. Show inspection information on a laptop dashboard
6. Store inspection results for later analysis

None of these should be called finished until reliable three-class performance, real-world testing and stable hardware are shown.
