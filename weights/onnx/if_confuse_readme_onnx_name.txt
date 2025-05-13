# Use for onnx inference

205_final_output_best_acc.onnx : Text detection for car license plates
205_rec_final.onnx : Text recognition for car license plates (use dict ppocr_keys_v1.txt, this is general version, with rec both chinese and english. This for fallback, else for normal, use english version named below.)
281_3_dein_rec_en_train2infer.onnx : Text recognition for car license plate (use dict en_dict.txt)
