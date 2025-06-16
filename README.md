private fun fetchCert(csr: String): String {
        val client = OkHttpClient()

        val formBody = FormBody.Builder()
            .add("Mode", "newreq")
            .add("CertRequest", csr)
            .add("CertAttrib", "UserAgent:Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36 SberBrowser/27.0.0.0\r\n")
            .add("FriendlyType", "Сертификат сохраненного запроса (11.06.2025, 16:25:08)")
            .add("ThumbPrint", "")
            .add("TargetStoreFlags", "0")
            .add("SaveCert", "yes")
            .build()

        val certReq = Request.Builder()
            .url("https://testgost2012.cryptopro.ru/certsrv/certfnsh.asp")
            .post(formBody)
            .header("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7")
            .header("Accept-Language", "ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7")
            .header("Cache-Control", "max-age=0")
            .header("Connection", "keep-alive")
            .header("Content-Type", "application/x-www-form-urlencoded")
            .header("Origin", "https://testgost2012.cryptopro.ru")
            .header("Referer", "https://testgost2012.cryptopro.ru/certsrv/certrqxt.asp")
            .header("Sec-Fetch-Dest", "document")
            .header("Sec-Fetch-Mode", "navigate")
            .header("Sec-Fetch-Site", "same-origin")
            .header("Sec-Fetch-User", "?1")
            .header("Upgrade-Insecure-Requests", "1")
            .header("User-Agent", "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36 SberBrowser/27.0.0.0")
            .header("sec-ch-ua", "\"Chromium\";v=\"135\", \"Not-A.Brand\";v=\"8\"")
            .header("sec-ch-ua-mobile", "?0")
            .header("sec-ch-ua-platform", "\"Linux\"")
            .header("Cookie", "_ym_uid=1749484640280831139; _ym_d=1749484640; SESS81ade102ea2d49056b49f538d682bdbf=vsf66oboimnbcn8aomof0ikde1; ASPSESSIONIDQUCBDQDA=KDDGDIJAHLOFEKMFINJDBLLI")
            .build()

        val reqResult = client.newCall(certReq).execute().use { response ->
            response.body!!.string()
        }

        val reqId = Regex("\\?ReqID=([0-9]+)&").find(reqResult)!!.groups[1]!!.value

        val downloadReq = Request.Builder()
            .url("https://testgost2012.cryptopro.ru/certsrv/certnew.cer?ReqID=$reqId&Enc=b64")
            .header("Accept", "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7")
            .header("Accept-Language", "ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7")
            .header("Connection", "keep-alive")
            .header("Referer", "https://testgost2012.cryptopro.ru/certsrv/certfnsh.asp")
            .header("Sec-Fetch-Dest", "document")
            .header("Sec-Fetch-Mode", "navigate")
            .header("Sec-Fetch-Site", "same-origin")
            .header("Sec-Fetch-User", "?1")
            .header("Upgrade-Insecure-Requests", "1")
            .header("User-Agent", "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36 SberBrowser/27.0.0.0")
            .header("sec-ch-ua", "\"Chromium\";v=\"135\", \"Not-A.Brand\";v=\"8\"")
            .header("sec-ch-ua-mobile", "?0")
            .header("sec-ch-ua-platform", "\"Linux\"")
            .header("Cookie", "_ym_uid=1749484640280831139; _ym_d=1749484640; SESS81ade102ea2d49056b49f538d682bdbf=vsf66oboimnbcn8aomof0ikde1; ASPSESSIONIDQUCBDQDA=AGFGDIJAKONNFHIMBEBMHPMD")
            .build()
        println(reqResult)

        return client.newCall(downloadReq).execute().use { response ->
            response.body!!.string()
        }
    }
