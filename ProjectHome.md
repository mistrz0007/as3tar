Decided to create a tar (.tar) library for others that may need to read such files.

http://en.wikipedia.org/wiki/Tar_%28file_format%29
```
import com.conceptualideas.compression.tar.TarOutputArchive
var tar:TarOutputArchive = new TarOutputArchive();			
			
var ba:ByteArray = new ByteArray();
ba.writeUTFBytes("Hello World!!");
tar.add(ba, "hello_world/hello_world.txt"); // yes support folders also!!			
			
var tarData:ByteArray=tar.save();
fileRef = new FileReference();
fileRef.save(tarData, "hello_world.tar");
```

Or Read archive
```
var loader:ByteLoader = new ByteLoader();

var request:URLRequest = new URLRequest("hello_world.tar");
loader.addEventListener(Event.COMPLETE, completeHandler);
loader.load(request);

private function completeHandler(e:Event):void
		{
			var ba:IDataInput = e.target.data as IDataInput;
			var tar:TarArchive = new TarArchive();
			tar.setInput(ba);
			var entries:Vector.<ITarEntry> = tar.entries;
			var entry:ITarEntry;
			for each(entry in entries) {
				if (entry.isFolder()) {
					trace("Folder ::", entry.info.name, TarEntryFolder(entry).size);
					trace(" Files ::");
					for each(var file:ITarEntry in TarEntryFolder(entry).getEntries()) {
						trace("-- " + file.info.name,file.info.resolvedPath, file.info.checksum);
						trace("--" +file.info);
					}
				}else {
					trace(entry.info);
				}
			}
```