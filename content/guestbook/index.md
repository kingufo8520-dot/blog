<div style="max-width: 500px; margin: 20px 0; padding: 20px; border: 1px solid #ddd; border-radius: 8px;">
  <div style="margin-bottom: 12px;">
    <label for="gb-name" style="display: block; margin-bottom: 4px; font-weight: bold;">이름</label>
    <input type="text" id="gb-name" placeholder="이름을 입력하세요" style="width: 100%; padding: 8px; box-sizing: border-box; border: 1px solid #ccc; border-radius: 4px;" />
  </div>
  <div style="margin-bottom: 12px;">
    <label for="gb-message" style="display: block; margin-bottom: 4px; font-weight: bold;">메시지</label>
    <textarea id="gb-message" rows="4" placeholder="방명록 메시지를 남겨주세요!" style="width: 100%; padding: 8px; box-sizing: border-box; border: 1px solid #ccc; border-radius: 4px;"></textarea>
  </div>
  <button id="gb-submit" onclick="submitGuestbook()" style="padding: 10px 16px; background-color: #2b579a; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">
    방명록 남기기
  </button>
  <p id="gb-status" style="margin-top: 10px; font-size: 14px;"></p>
</div>

<script>
async function submitGuestbook() {
  const nameInput = document.getElementById("gb-name");
  const messageInput = document.getElementById("gb-message");
  const submitBtn = document.getElementById("gb-submit");
  const statusText = document.getElementById("gb-status");

  const name = nameInput.value.trim();
  const message = messageInput.value.trim();

  if (!name || !message) {
    statusText.style.color = "red";
    statusText.innerText = "이름과 메시지를 모두 입력해 주세요.";
    return;
  }

  submitBtn.disabled = true;
  statusText.style.color = "#555";
  statusText.innerText = "등록 중입니다... 잠시만 기다려 주세요.";

  try {
    const response = await fetch("https://hello-world-dry-sun-3fd9.kingufo8520.workers.dev/", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ name, message }),
    });

    const result = await response.json();

    if (response.ok && result.success) {
      statusText.style.color = "green";
      statusText.innerText = "성공적으로 등록되었습니다! (블로그 반영까지 약 1~2분 소요됩니다)";
      nameInput.value = "";
      messageInput.value = "";
    } else {
      statusText.style.color = "red";
      statusText.innerText = "등록 실패: " + (result.error || "알 수 없는 오류");
    }
  } catch (error) {
    statusText.style.color = "red";
    statusText.innerText = "네트워크 오류가 발생했습니다: " + error.message;
  } finally {
    submitBtn.disabled = false;
  }
}
</script>
