<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>ডুপ্লিকেট নাম চেকার</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    textarea { width: 100%; height: 200px; }
    button { margin-top: 10px; padding: 10px 20px; cursor: pointer; }
    .result { margin-top: 20px; }
    .name-block { border: 1px solid #ccc; padding: 10px; margin-bottom: 10px; background: #f9f9f9; }
  </style>
</head>
<body>
  <h2>আপনার লিস্ট দিন</h2>
  <textarea id="listInput" placeholder="এখানে নামের লিস্ট পেস্ট করুন..."></textarea>
  <br>
  <button onclick="checkDuplicates()">চেক করুন ✅</button>

  <div class="result" id="result"></div>

  <script>
    function checkDuplicates() {
      const input = document.getElementById("listInput").value.trim();
      const lines = input.split("\n").map(line => line.trim()).filter(line => line);

      const nameMap = {};

      lines.forEach((line, index) => {
        // ➤ দিয়ে নাম আলাদা করা হলে সেটি ধরবে
        const parts = line.split("➤");
        const name = parts.length > 1 ? parts[1].trim() : line;

        if (!nameMap[name]) {
          nameMap[name] = { count: 0, positions: [] };
        }
        nameMap[name].count++;
        nameMap[name].positions.push(index + 1);
      });

      const resultDiv = document.getElementById("result");
      resultDiv.innerHTML = "";

      if (Object.keys(nameMap).length === 0) {
        resultDiv.innerHTML = "<p>⚠️ কোনো নাম পাওয়া যায়নি!</p>";
        return;
      }

      let foundDuplicate = false;

      for (const [name, data] of Object.entries(nameMap)) {
        if (data.count > 1) { // শুধু ডুপ্লিকেট দেখাবে
          foundDuplicate = true;
          const block = document.createElement("div");
          block.className = "name-block";
          block.innerHTML = `
            <strong>নাম:</strong> ${name} <br>
            <strong>কতবার এসেছে:</strong> ${data.count} <br>
            <strong>পজিশন:</strong> ${data.positions.join(", ")}
          `;
          resultDiv.appendChild(block);
        }
      }

      if (!foundDuplicate) {
        resultDiv.innerHTML = "<p>✅ কোনো ডুপ্লিকেট নাম পাওয়া যায়নি!</p>";
      }
    }
  </script>
</body>
</html>
