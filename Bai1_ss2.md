Chọn phương án B, vì đây là phương án duy nhất tổ chức cấu hình đúng theo Spring Profiles, có tính đóng gói cao và dễ bảo trì.

Tuy nhiên, để phương án B hoàn chỉnh về kỹ thuật, nên:

Dùng spring.ai.model.chat=ollama trong profile local.
Dùng spring.ai.model.chat=openai trong profile cloud.
Không hard-code spring.profiles.active=local; nên chọn profile bằng biến môi trường hoặc tham số khởi động.
Giữ OPENROUTER_API_KEY trong biến môi trường hoặc secret manager, không ghi trực tiếp vào source code.
