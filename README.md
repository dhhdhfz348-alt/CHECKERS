<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>Shashka: Online O'yinchilar</title>
    <style>
        body { background: #0f172a; color: white; font-family: sans-serif; padding: 10px; }
        .container { max-width: 400px; margin: auto; }
        .online-list { background: #1e293b; padding: 15px; border-radius: 12px; }
        .user-item { display: flex; align-items: center; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #334155; }
        .user-info { display: flex; align-items: center; gap: 10px; }
        .avatar { width: 35px; height: 35px; background: #6366f1; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 14px; }
        .btn-invite { background: #22c55e; border: none; padding: 5px 10px; color: white; border-radius: 5px; cursor: pointer; }
    </style>
</head>
<body>
<div class="container">
    <h2>Abdurahmon Tog'ajon</h2>
    <h3>Online o'yinchilar:</h3>
    <div id="online-users" class="online-list"></div>
</div>

<script>
    const names = [
        "Jasurbek", "Dilshod", "Azamat", "Bekzod", "Umidjon", "Otabek", "Sardor", "Sherzod",
        "Nuriddin", "Firdavs", "Muhammad", "Rustam", "Islom", "Bobur", "Kamron", "Shoxrux",
        "Zafar", "Sanjar", "Akmal", "Qobiljon Yalpak"
    ];

    function renderUsers() {
        const list = document.getElementById('online-users');
        list.innerHTML = "";
        names.forEach(name => {
            list.innerHTML += `
                <div class="user-item">
                    <div class="user-info">
                        <div class="avatar">${name[0]}</div>
                        <span>${name}</span>
                    </div>
                    <button class="btn-invite" onclick="invite('${name}')">Taklif</button>
                </div>`;
        });
    }

    function invite(name) {
        alert(name + "ga taklif yuborildi. U javob berishini kuting...");
    }

    // 1 kunda bir marta xabar chiqarish (bu yerda test uchun har 10 soniyada)
    setInterval(() => {
        const randomName = names[Math.floor(Math.random() * names.length)];
        alert(randomName + "dan xabar: 'Kel birga shashka o'ynab dam olamiz?'");
    }, 86400000); // 86400000ms = 24 soat

    renderUsers();
</script>
</body>
</html>
