```javascript
const listCodeSub = {
  'PHI1002-8': 'LmJTDm8fqUVRu3muu48ODg',
  'INS2080-2': 'DL_83DG0YUU278hpTFH4oA',
  'INS3078-2': 'U8aJQKNap_GfxcIW54UT8A',
  'INS3254-4': 'M6PI5nOxRG76_IQanX47FQ',
  'INS2015-2': 'ELCV_ZHutkobHUvyJ14P2w',
  'INS2004-1': '011HgbXTqlPWLHgs8h0EsQ',
  'PES1025-1': 'Vr1ogIY_lDwfzi5d0SBEuw',
  'SOC1050-2': '9RiP5FUKsSXIOoJSBD9Wkg',
  'ISV1020-17': 'qRN4nk9PeoQ_zY5QimKuVg',
  'ISV1023-2': 'MErboVUtxxbV9NmisphvmQ',
};

const buildReqRegisterV1 = async (code, key) => {
  const data = new URLSearchParams({
    'param[IDDotDangKy]': '1039',
    'param[IDLoaiDangKy]': '1',
    'param[GuidIDLopHocPhan]': code,
  }).toString();

  try {
    const responseData = await fetch('https://sv.isvnu.vn/dang-ky-hoc-phan.html', {
      method: 'POST',
      headers: {
        authority: 'sv.isvnu.vn',
        accept: 'application/json, text/javascript, */*; q=0.01',
        'accept-language': 'en-US,en;q=0.9',
        'content-type': 'application/x-www-form-urlencoded; charset=UTF-8',
        cookie:
          'ASP.NET_SessionId=3rlxfzehzhwwnmtkz1y3zkz4; __RequestVerificationToken=uOunzacoSlfzfDm7q0hecetSz5WQSsjgeEvfUBxcgc9ByvNNseu3YyRaA7VMTIRRjTu6lDHcTPi6aTKmGRNbBVbls3Tk6_Di0sW_uSahCWg1; PAVWs3tE769MuloUJ81Y=Q3X4ccannyJpIPWITWQw3T8iXcllbtDVFVjJhIuH3RM; ASC.AUTH=755A497F546B9D281732A5AF44757F7E119DD1315497203E6837DC1F4215B945184294E75A4625F82A2EC2AD046CCB38CF83E3772330BC8417722F97976E9F1E3A9B9B236E95C149687527B3B0518AD9528B6A90C38F5B8E61A28558A66FC3AC943A0031E0F7C4E307DF2A8F1F54442B12847F9FD480468623FFAA76AA4FED71',
        origin: 'https://sv.isvnu.vn',
        referer: 'https://sv.isvnu.vn/dang-ky-hoc-phan.html',
        'sec-ch-ua': '"Not_A Brand";v="8", "Chromium";v="120", "Microsoft Edge";v="120"',
        'sec-ch-ua-mobile': '?0',
        'sec-ch-ua-platform': '"Linux"',
        'sec-fetch-dest': 'empty',
        'sec-fetch-mode': 'cors',
        'sec-fetch-site': 'same-origin',
        'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0',
        'x-requested-with': 'XMLHttpRequest',
      },

      // body: JSON.stringify({
      // 'param[IDDotDangKy]': "1039",
      // "param[IDLoaiDangKy]": "1",
      // "param[GuidIDLopHocPhan]": code
      // }),

      body: data,
    });

    const contentType = responseData.headers.get('content-type');

    if (contentType && contentType.includes('application/json')) {
      const json = await responseData.json();
      console.log(`Code - ${key}`, json);
      return true;
    }

    console.log(`Error - timeout - ${key}`);

    return false;
  } catch (error) {
    console.log(`Error ${key}: `, error);

    return false;
  }
};

const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

let start = Date.now();

const runRegister = async () => {
  for (const [key, value] of Object.entries(listCodeSub)) {
    const isSuccess = await buildReqRegisterV1(value, key);

    if (isSuccess) {
      console.log(`code ${key}: `, value);
      delete listCodeSub[key];
      const duration = Date.now() - start;
      start = Date.now();
      console.log('🚀 ~ runRegister ~ duration:', duration);
    }

    await delay(5000);
  }
};

const loopRegister = async () => {
  console.log('🚀 ~ loopRegister ~ listCodeSub:', listCodeSub);
  await runRegister();
  if (Object.keys(listCodeSub).length !== 0) {
    await loopRegister();
  }
};

loopRegister();


```