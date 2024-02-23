# solucion-onFocus-OnBlur-en-expo-y-react-native-para-android-tv
solucion onFocus/OnBlur en expo y react native para android tv

Esta es la solucion que yo encontre para que onFocus y onBlur funcionara en android tv con expo y react native utilizando
react-native-tvos, escribo esto porque me costo mucho hallar una solucion y la describo aqui para que otros la hallen igual que yo

<b>paso 1)</b>
  cambiar el nombre a la carpeta node_modules

<b>paso 2)</b> 
  agrega "react-native": "npm:react-native-tvos@latest", tal como lo dice su documentacion en https://www.npmjs.com/package/react-native-tvos?activeTab=readme
  y asegurate que no tienes en tu archivo package.json una linea como esta  "react-native-tvos": "^0.73.4-0", si hay una linea a si en tu package.json borrala
  luego coloca este comando yarn install o npm install

paso 3) 
  asegurate que de activar esta opcion newArchEnabled=true en el archivo gradle.properties en la carpeta tuApp/android/gradle.properties

<b>paso 4)</b> 
  onFocus y onBlur no funciona con TouchableWithoutFeedback, TouchableOpacity, TouchableHighlight solo funciona con Pressable pero solo despues 
  de activar newArchEnabled=true en gradle.properties

<b>paso 5)</b> 
 ejemplo de codigo 

 
  return (
      <View style={styles.container}>
          <StatusBar style="light" backgroundColor="rgb(44,44,44)" />

          <Image source={require('../assets/fondo_b.jpg')} style={styles.imagen} />
          <Image source={require('../assets/logo-big.png')} style={{width:'80%',marginBottom:100,resizeMode:'contain'}} />
          
          <Pressable    
					hasTVPreferredFocus={true}
					autofocus={true}
                    onFocus ={ () => {
                      settextoFocob( true );
                      console.log("sii entro sadsad");
                    } }
                    onBlur={ () => {
                      settextoFoco( false );
					  settextoFocob( false );
                    } }
                    onPress={
                      () => {
                        textoUsuario.current.focus();
                      }
                    }
                      style={{width:'100%'}}
                    >
              <View style={{width:'100%',alignItems: 'center',justifyContent: 'center'}}>
                  <TextInput
                    ref={textoUsuario}
                    accessible={true}
                    onChangeText={ (text) => { vs.usuario = text; }}
                    placeholder="Usuario"
                    placeholderTextColor="#ffffffaa"
                    onBlur={ () => {
                      settextoFoco( false );
					  settextoFocob( false );
                    } }
                    style={textoFocob? styles.texto1Hover : styles.texto1 }
                  />
                <Image source={require('../assets/USUARIO.png')} style={{width:25,height:25,resizeMode:'contain',position:'absolute',left:'13%'}} />
                  
              </View>
          </Pressable>

          <Pressable                     
                    onFocus={ () => {
                      settexto2Foco( true );
                      console.log("sii entro 2222");
                    } }
                    onBlur={ () => {
                      settexto2Foco( false );
                    } } 
                    onPress={
                      () => {
                        textoClave.current.focus();
                      }
                    }
                    style={{width:'100%'}}
                  >
              <View style={{width:'100%',alignItems: 'center',justifyContent: 'center'}}>
                  <TextInput
                    ref={textoClave}
                    accessible={true}
                    secureTextEntry={true}
                    onChangeText={ (text) => { vs.clave = text; }}
                    placeholder="Contraseña"
                    placeholderTextColor="#ffffffaa"

                    style={texto2Foco? styles.texto2Hover : styles.texto2 }
                  />
                  <Image source={require('../assets/Inicio.png')} style={{width:25,height:25,resizeMode:'contain',position:'absolute',left:'13%',top:'50%'}} />
                    
              </View>
          </Pressable>


          <Pressable
              accessible={true}
              
              onFocus={ () => {
                setBoton0( true );
              } }
              onBlur={ () => {
                setBoton0( false );
              } }
              style={ boton0? styles.botonHover01 : styles.boton01 }
              onPress={() => {
                
                const intLog = async () => {

                    const rlog = await logearce(vs.usuario,vs.clave);
                    
                    if( rlog == true ) { 
                      await guardarDatos("usuario",vs.usuario);
                      await guardarDatos("clave",vs.clave);
                      setPantallaActual( vs.listaCanales );
                    }
                }
                intLog();

              }}
            >
              <View style={{alignItems: 'center'}}>
                <Text style={{ fontWeight: 'bold', fontSize:18, color: 'white' }}>LOGIN</Text>
              </View>
            </Pressable>

      </View>
  );
